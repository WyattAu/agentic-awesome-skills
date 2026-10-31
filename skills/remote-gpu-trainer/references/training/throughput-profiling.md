# Throughput & profiling: make training FAST, find the one bottleneck
right speedup in cost order; from a free dataloader knob to `torch.compile` and fused attention. This
> **Size the run to the box; then PIN it for any comparison.** Auto-sizing batch/`num_workers` to the
> or baseline-vs-variant comparison, **pin the same batch across all cells**; auto-maximizing per-box
- **Diagnose first**; T1 the 3-way split (GPU/data/comms-bound) · T2 util%-is-a-liar pointer · T3 the cheap CPU/GPU-busy triage
- **Dataloader (the #1 cause of a starved GPU)**; T4 num_workers · T5 persistent_workers · T6 pin_memory + non_blocking · T7 prefetch_factor · T8 IO-bound vs CPU-transform-bound
- **Free / near-free knobs**; T9 TF32 + matmul precision · T10 cudnn.benchmark · T11 channels_last · T12 set_to_none + disable debug APIs
- **Mixed precision for speed**; T13 bf16/fp16 throughput
- **Kernels**; T14 SDPA / FlashAttention · T15 torch.compile gains · T16 torch.compile recompilation traps
- **Memory↔speed trades**; T17 activation checkpointing speed cost · T18 batch size vs throughput
- **Profilers**; T19 torch.profiler (is-it-data-bound) · T20 nsys / Nsight Systems · T21 py-spy (live, no restart) · T22 memory-snapshot pointer
- **Multi-GPU / multi-node comms**; T23 DDP/FSDP compute-comm overlap
- **Pointers**; gotchas_universal.md U8/U21/U24/U25/U38 · oom-memory.md · distributed-launch.md · multinode.md · verifying-dl-experiments (skill)
## Diagnose first: do NOT tune blind
### T1: The 3-way split: GPU-bound vs data-bound vs comms-bound (decide before touching a knob)
**Fix; classify with one cheap reading each** (heuristic: util consistently >90% ⇒ GPU-bound;
low/fluctuating ⇒ elsewhere; both CPU+GPU low ⇒ I/O -
The highest-signal instrument is a **profiler trace** (T19); read it before changing anything.
### T2: `nvidia-smi` GPU-Util % lies; correlate clock + power → gotchas_universal.md U21
A 100%-util tile can hide a starved GPU (a trickle of tiny kernels reads as 100%). The full diagnosis -
thermal/power-throttle slowdown; lives in **gotchas_universal.md U21/U23**; read it before concluding a run
### T3: Cheap triage when no profiler is wired yet: is the host CPU busy?
**Fix**: watch GPU and CPU at once for ~10 s -
cores (not one worker pegged) ⇒ intra-op thread oversubscription** on a vCPU slice, not data-bound; cap
## Dataloader: the #1 reason a rented GPU sits idle
### T4: `num_workers`: 0 means the main process loads serially (the default starves the GPU)
**Symptom**: `DataLoader(num_workers=0)` (the default); every batch is fetched on the main thread, GPU
**Root cause**: with `num_workers=0` "the data will be loaded in the main process"; no overlap between
### T5: `persistent_workers=True`: stop paying worker-startup every epoch
once and **re-forks them next epoch**; re-importing, re-opening files, rebuilding the dataset object each
### T6: `pin_memory=True` + `non_blocking=True`: overlap the host→device copy
`x = x.to(device, non_blocking=True)` to reduce host blocking. This does not automatically create a separate CUDA stream or overlap transfer with compute: use pinned buffers, a separate stream and correct synchronization/lifetime management for actual device overlap. Non-blocking pageable copies have different staging behavior; measure rather than assuming a universal synchronous fallback. Costs host RAM (pinned pages aren't swappable); back off if it pressures the cgroup (U9).
### T7: `prefetch_factor`: deepen the queue when fetch time is bursty
**Symptom**: with workers on, the GPU still periodically stalls; every *Nth* step (N = `num_workers`) has
**Root cause**: `prefetch_factor` defaults to **2** when `num_workers>0` (None when 0); "2 means there
**Fix**: raise `prefetch_factor` (3–4) so workers run ahead and bursts hide; at the cost of more resident
the GPU's consume rate, no depth helps; fix the rate (workers T4, GPU transform T8, NVMe U8) instead.
### T8: IO-bound vs CPU-transform-bound are different data-bound cases (different fix)
**Root cause; split the case**:
**Fix**: read the trace (T19); time in `read`/`stat` ⇒ U8/U25; time in a transform fn ⇒ move to GPU.
### T9: TF32 / `set_float32_matmul_precision("high")`: the "why is my A100 slow" footgun
### T10: `cudnn.benchmark=True`: autotune conv algorithms (fixed input shapes only)
stable**; with variable shapes (dynamic resolution, ragged batches) it re-benchmarks every new shape and
determinism knobs; whether to enable it for a clean datapoint is owned by precision-stability P19 /
### T11: `channels_last`: free Tensor-Core speedup for conv nets under AMP
**Fix**: convert model and inputs to `memory_format=torch.channels_last` -
### T12: `set_to_none` + disable debug APIs (two free per-step taxes to remove)
- **`optimizer.zero_grad(set_to_none=True)`** (the **default** since PyTorch 2.0) over zero-filling -
- **Turn OFF debug APIs for the real run**; `torch.autograd.set_detect_anomaly(True)`,
### T13: bf16/fp16 is a throughput lever, not just a memory lever
read/write bandwidth; a speedup *on top of* the memory saving (oom-memory.md M6).
**Fix**: `torch.autocast("cuda", dtype=torch.bfloat16)` on Ampere+ (the modern default; no GradScaler -
## Kernels: the levers left once the GPU is fed
### T14: SDPA / FlashAttention: stop materializing the O(seq²) attention matrix
(wrong dtype, head dim, mask shape); "if a fused implementation is not available, a warning will be
  which auto-picks FlashAttention / memory-efficient / cuDNN / math. Feed it **fp16/bf16** inputs; the
  `<96 GB` RAM ninja over-parallelizes and OOMs the build; cap `MAX_JOBS=4 pip install flash-attn
### T15: `torch.compile`: fuse kernels + cut launch overhead (one line, real gains)
**Fix**: wrap the model; `model = torch.compile(model)`. Modes
- `default`; balanced speed/memory.
- `mode="reduce-overhead"`; uses **CUDA graphs** to kill Python overhead (best for many tiny ops /
- `mode="max-autotune"`; longest compile, fastest steady-state.
- compilation is lazy on first call (https://huggingface.co/docs/transformers/en/perf_torch_compile); exclude
### T16: `torch.compile` recompilation trap: variable shapes silently blow the cache → eager
`cache_size_limit`) Dynamo **stops compiling that function and runs it eagerly**; paying all the compile
 ; `torch.compile(model, dynamic=True)` (or `mark_dynamic` / `TORCH_COMPILE_DYNAMIC_SOURCES`) compiles
  legitimately exist; raising it to mask genuinely unbounded shapes just thrashes.
### T17: Activation checkpointing buys memory by spending ~20–30% compute (know the cost)
**Symptom**: gradient/activation checkpointing is on "to be safe" and training is slow; but the model
**Fix**: checkpointing **recomputes** activations in backward instead of storing them; trading **~20–30%
### T18: Bigger micro-batch ≈ better GPU utilization (up to the memory wall)
result depends on it (`batch 4 × accum 16` beats `batch 1 × accum 64`; oom-memory.md M5). Accuracy/effective-
## Profilers: measure the bottleneck, don't guess it
### T19: `torch.profiler`: the definitive data-bound vs compute-bound verdict
**Fix; scheduled profile of a few steps**
overlapped ⇒ comms-bound (T23). On a remote box write the trace and view locally; for raw
### T20: `nsys` / Nsight Systems: system-wide timeline when the gap is below PyTorch's view
**Root cause**: torch.profiler sees PyTorch ops; `nsys` traces the whole system; CUDA API, kernels,
memcpy, NVTX ranges, OS-runtime; so it exposes launch-bound stalls and CPU↔GPU sync that PyTorch can't.
"Periodic gaps in the CUDA HW row are moments when the GPU is idle; a red flag"
**Fix; profile a bounded window on the box, view locally** (canonical PyTorch recipe,
Nsight **Compute** (`ncu`) finds *why* (occupancy, bandwidth, warp stalls); but `ncu` is heavy, reserve it
### T21: `py-spy`: profile a LIVE training process with no restart, no code change
**Fix; attach by PID, zero instrumentation** (https://github.com/benfred/py-spy):
shows sampled stacks; repeated samples plus progress/artifact checks help distinguish waiting, slow work and a hang (one stack alone is not conclusive); pairs with the "is it actually hung?" check (gotchas_universal.md U17, verifying-dl-experiments
### T22: CUDA memory snapshot/visualizer → oom-memory.md M19
memory tool, not a throughput tool; listed here only so the profiler menu is complete. Do NOT restate.
### T23: Compute-comms overlap: DDP overlaps by default; tune the bucket, watch for breakers
**Symptom**: scaling efficiency is poor; per-GPU util high, but N GPUs deliver far less than N× throughput;
**Fix (single box, DDP/FSDP; the launch/sharding mechanics live in
straggler, MTU mismatch) is **references/multinode.md** (**REQUIRED** for ≥2 instances); a comms "slowdown"
## Pointers: throughput gotchas catalogued elsewhere (do NOT restate)
- **gotchas_universal.md**; **U8** stage hot data to local NVMe (IO-bound) · **U21** `nvidia-smi` util% is
- **references/training/oom-memory.md**; M5 micro-batch/grad-accum · M6 bf16 activations · M7 activation
- **references/training/precision-stability.md**; P1–P10 the precision decision + AMP mechanics · P2 the
- **references/training/distributed-launch.md**; torchrun/Accelerate/DeepSpeed launch, DDP/FSDP sharding,
- **references/multinode.md**; inter-node NCCL/NIC/fabric/timeout/MTU (the wire between boxes). Single-box
- **verifying-dl-experiments** (**REQUIRED**); owns *is-the-number-real*: whether a kernel/precision/compile