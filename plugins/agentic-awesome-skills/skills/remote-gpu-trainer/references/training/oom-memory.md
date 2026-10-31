# OOM & fitting a model that doesn't: VRAM + host-RAM out-of-memory during training
activations vs fragmentation), and apply the fixes **in cost order**; from a free batch-size cut to
- **Read it first**; M1 anatomy (where VRAM goes) · M2 reading the OOM trace · M3 VRAM-OOM vs host-RAM-OOM
- **Fixes in order**; M4 the ladder (do these top-down) · M5 batch/grad-accum · M6 bf16 mixed precision · M7 activation/gradient checkpointing · M8 expandable_segments · M9 FSDP / ZeRO sharding · M10 CPU/NVMe offload · M11 seq-len/resolution · M12 8-bit & paged optimizers · M13 LoRA/QLoRA
- **OOM at a specific step**; M14 first backward · M15 validation/eval · M16 the longest batch · M17 step-2 (optimizer alloc)
- **Debugging**; M18 memory_summary · M19 the snapshot + visualizer · M20 empty_cache & "leak" myths
- **Pointers**; host-RAM cgroup-OOM → gotchas_universal.md U9 · VRAM-vs-cgroup → U10 · zombie-VRAM → U11
### M1: Anatomy: where the VRAM actually goes (a model that runs inference OOMs in training)
### M2: Reading the CUDA OOM trace (the numbers tell you which fix)
**Root cause; decode the four numbers**:
- **Tried to allocate X**; the size of the *single* failing request. Large X = a big tensor (long-seq
- **reserved B vs allocated Z**; `reserved` = total the caching allocator grabbed from the driver;
- **free A**; driver-visible free on the card; if A is large but the alloc still fails, suspect another
### M3: VRAM OOM is not host-RAM OOM (distinct failure, distinct fix)
(cgroup `memory.max`) exhaustion; almost always `num_workers × a big in-RAM object`. These have opposite
### M4: The ladder: apply top-down, stop when it fits
1. **Reduce micro-batch + grad-accumulation** (M5); preserves nominal effective batch, but check normalization, dropout, loss weighting and scheduler behavior.
2. **bf16 mixed precision** (M6); halves activations, usually a speedup; bf16 needs no loss scaling.
3. **Activation / gradient checkpointing** (M7); trades ~20–30% compute for a large activation cut.
4. **`PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True`** (M8); free; fixes *fragmentation* OOM only.
5. **SDPA / FlashAttention**; stop materializing the full O(seq²) attention matrix (M11).
6. **Shorter seq-len / lower resolution** (M11); cheap but **changes the science** → verify (REQUIRED).
7. **8-bit / paged optimizer** (M12); cuts the 8 B/param optimizer state to ~2 B with ~no accuracy loss.
8. **FSDP / DeepSpeed ZeRO-1→2→3** (M9); shard optimizer→grads→params across GPUs (needs ≥2 GPUs).
9. **CPU / NVMe offload** (M10); last resort on a single GPU; trades a large speed hit for fit.
10. **LoRA / QLoRA** (M13); for *finetuning*: freeze base, train adapters; QLoRA quantizes base to 4-bit.
### M5: Reduce micro-batch + gradient accumulation (the free first move)
sub-batches before one optimizer step; same math, lower peak activation memory. Keep micro-batch as large
as fits (batch 4 × accum 16 beats batch 1 × accum 64; better GPU utilization).
sub-batch token counts; a correctness issue owned by **verifying-dl-experiments** (REQUIRED).
### M6: bf16 mixed precision (prefer bf16 over fp16 on Ampere+)
loss-scaling for ordinary bf16 AMP, but can still produce non-finite values or lose small updates; fewer NaN failures. Note fp16 can *increase* memory at small
### M7: Activation / gradient checkpointing (trade compute for activation memory)
**recomputed** during backward; cuts activation memory substantially at **~20–30% slower** training.
DDP, reentrant checkpointing can break; use `use_reentrant=False`.
### M8: `expandable_segments:True` (the free fragmentation fix)
instead of each `cudaMalloc` being an unmergeable block; which is the root of fragmentation.
same knob referenced in **gotchas_universal.md U10**; set it as a default on the box, it is nearly free.
allocators (e.g. NCCL `ncclMemAlloc`, pytorch/pytorch#165419); if a custom-allocator stack misbehaves,
### M9: FSDP / DeepSpeed ZeRO sharding (≥2 GPUs: shard the 18 B/param)
**Fix; shard training state across the data-parallel group**, escalating by stage:
- **ZeRO-1 / `optim_state_dict` shard**; partition **optimizer states** (the 8 B/param). Smallest comms
- **ZeRO-2 / FSDP `SHARD_GRAD_OP`**; also partition **gradients** (the 4 B). Good memory/comms balance.
- **ZeRO-3 / FSDP `FULL_SHARD`**; also partition **parameters** (the 6 B). Maximum savings; needs
### M10: CPU / NVMe offload (single-GPU last resort)
**ZeRO-Infinity** offloads to both for huge models. QLoRA's **paged optimizers** (M12) are a lighter form -
(https://www.deepspeed.ai/2021/03/07/zero3-offload.html). Cost: PCIe/NVMe bandwidth becomes the bottleneck -
### M11: Reduce sequence length / resolution / attention footprint
- **Use SDPA / FlashAttention** to avoid materializing the full seq² attention matrix -
- Only then **shorten seq-len / lower image resolution / patchify**; this **changes the task/science**;
### M12: 8-bit & paged optimizers (cut the 8 B/param optimizer state to ~2 B)
**Fix**: swap AdamW for a **quantized** optimizer; HF `optim="adamw_bnb_8bit"` / `"paged_adamw_8bit"`
### M13: LoRA / QLoRA (finetuning only: don't train the full model)
optimizers), train fp16/bf16 adapters on top; reported to finetune a **65B model on a single 48 GB GPU**
capacity*; it is a different optimization target, not a free OOM trick. Whether the LoRA result matches
### M14: OOM on the **first backward** (not the forward)
**gradient** buffers (4 B/param) and needs every cached activation live simultaneously; peak memory is at
**Fix**: M7 (checkpointing; recompute instead of store activations) is the targeted fix; then M5/M6. If the
### M15: OOM only during **validation / eval**, training was fine
**Symptom**: training epochs run; the first eval pass OOMs; even with `torch.no_grad()` / `model.eval()`,
**Root cause; two distinct ones**:
   the GPU** for the whole eval set before moving to CPU; a large eval set OOMs regardless of batch size
### M16: OOM mid-epoch on the **longest batch** (variable-length / bucketed data)
Don't conclude "data corruption" from a step-N OOM; it's the longest batch.
### M17: OOM on **step 2** (after the first optimizer step), step 1 fine
**Fix**: budget for the **post-step** peak, not step-1; measure peak with `max_memory_allocated()` *after*
### M18: `torch.cuda.memory_summary()` + the stat functions (first look)
tensors) vs `torch.cuda.memory_reserved()` (allocator total); a big gap = fragmentation/caching (→ M8);
### M19: The CUDA memory snapshot + visualizer (find the exact culprit allocation)
**Symptom**: summary stats aren't enough; need *which line of code* allocated the memory that OOMs.
**Fix; record a snapshot around the OOM and view the timeline**:
(references/ssh_transport.md), view locally; don't try to run the visualizer over ssh.
### M20: `empty_cache()` and the "memory leak" myths
  cost (HF `torch_empty_cache_steps=N` runs it every N steps -
  plateauing is normal. A *true* leak is **rising `memory_allocated()`** across steps with constant batch -
## Pointers: memory gotchas catalogued elsewhere (do NOT restate)
  weighting, determinism) → **verifying-dl-experiments** (REQUIRED; this layer makes it *fit and run*; that