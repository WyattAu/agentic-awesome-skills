# Launching & debugging multi-GPU / multi-node training: torchrun · Accelerate · DeepSpeed · DDP · FSDP
and; when 8 processes silently freeze; find *which* rank diverged. This layer owns *making the
for any job spanning ≥2 instances); this file ends where the wire between boxes begins.
- **Launchers & env**; D1 torchrun-env-contract · D2 standalone-vs-rendezvous · D3 LOCAL_RANK-device-bug · D4 port-collision · D5 accelerate-launch · D6 deepspeed-launcher · D7 which-launcher
- **DDP**; D8 find_unused_parameters · D9 uneven-inputs-Join · D10 SyncBN-&-buffers · D11 effective-batch/LR
- **FSDP**; D12 wrapping-policy · D13 sharding-strategy · D14 mixed-precision · D15 state_dict-type
- **DeepSpeed**; D16 ZeRO-stages · D17 config.json-knobs · D18 auto-&-engine.backward
- **The HANGS** (highest-value); D19 desync-debug-toolkit · D20 one-rank-diverged · D21 rank-conditional-collective · D22 dataloader-length-mismatch · D23 eval/print/save-on-one-rank
- **Pointers**; inter-node NCCL/NIC/timeout → multinode.md · OOM/sharding-to-fit → oom-memory.md · spot-restart → spot-resilience.md
### D1: The rank/world-size env contract every launcher must satisfy
| `LOCAL_RANK` | rank **within this node**; bind it to the GPU (`cuda:LOCAL_RANK`), NOT `RANK` (D3) |
### D2: Single-node uses `--standalone`; multi-node needs a shared rendezvous id+endpoint
# single node, 4 GPUs: self-contained, no addr/port to manage
### D3: Every process lands on GPU 0 (the `RANK` vs `LOCAL_RANK` bug)
### D4: `RuntimeError: Address already in use` when launching a second job on one node
### D5: HF Accelerate: `accelerate launch` reads a config, not torchrun flags
**Fix**: generate a config once, then launch against it; and on a headless rental, write the YAML
### D6: DeepSpeed: `deepspeed` launcher vs `accelerate launch`, and the `hostfile`
via `--config_file` and let it spawn the workers; don't mix both launchers.
### D7: Which launcher / parallelism: decision in one breath
### D8: `find_unused_parameters`: the "Expected to have finished reduction" error vs the silent hang
**Fix; in priority order**:
2. If a branch is *legitimately* unused some steps, `DDP(model, find_unused_parameters=True)`; but it adds a full graph traversal each step and **can be drastically slower** ([PyTorch forum](https://discuss.pytorch.org/t/process-got-stuck-when-set-find-unused-parameters-true-in-ddp/106078)). Use only if (1) is impossible.
3. If the return value is a dict/list, DDP may not locate the output tensors; flatten or simplify the `forward` return.
> Setting `find_unused_parameters=True` to *paper over* a real bug masks it; confirm the params are intentionally unused, don't silence the diagnostic.
### D9: Ranks have unequal batch counts → hang at the last step (uneven inputs)
filter, or a `IterableDataset` can leave counts uneven; the short rank stops calling allreduce.
- Truly uneven inputs (variable-length, can't pad): wrap the loop in the **Join** context manager -
 ; which mirrors the missing ranks' collectives so finished ranks don't deadlock
  silent correctness bug; **verifying-dl-experiments** **REQUIRED**).
### D10: BatchNorm stats diverge across ranks; buffers aren't synced
**Symptom**: DDP converges worse than single-GPU at the same effective batch, or eval is unstable -
### D11: N GPUs silently N× the effective batch (and the LR is now wrong)
`world_size`, per-GPU batch, and effective batch in the run manifest. **This changes the science** -
### D12: FSDP wraps the whole model as one unit → no memory saving (wrapping policy)
**Root cause**: with no `auto_wrap_policy`, FSDP makes the **entire model one FSDP unit**; it must
### D13: Sharding strategy: FULL_SHARD vs SHARD_GRAD_OP vs HYBRID
params+grads+optimizer; max memory saving, max comms. `SHARD_GRAD_OP` (== ZeRO-2) shards grads+optim
only, keeps params resident; less comms, more memory.
**Fix**: pick by the binding constraint; OOM → `FULL_SHARD`; comms-bound but it fits →
### D14: FSDP mixed precision: loss diverges or buffers stay fp32
reduce_dtype, buffer_dtype)`; not a single AMP flag. Setting `param_dtype=bf16` but leaving
**Fix**: set all three deliberately; a safe default is `param_dtype=bf16, reduce_dtype=fp32` (keep
### D15: Checkpoint OOMs or saves an unloadable shard (state_dict type)
- Large models / want resumable-at-scale: **`SHARDED_STATE_DICT`** via Distributed Checkpoint (DCP); each rank saves its shard, reload reshards to any world size.
### D16: ZeRO stage selection (1/2/3) and what each shards
**Fix**: smallest stage that fits; Stage 2 is the common sweet spot for models that *almost* fit;
### D17: The `ds_config.json` knobs that actually matter
**Fix**; the load-bearing keys:
`offload_*_device`, and `mixed_precision` from the Accelerate config are **overridden by the JSON** -
### D18: `"auto"` mismatch and `loss.backward()` vs `engine.backward()`
scaling lives inside the engine; calling bare `loss.backward()` instead of `model_engine.backward(loss)`
loop call `model_engine.backward(loss); model_engine.step()`; never `loss.backward()` /
## The HANGS: debugging a frozen distributed job (highest-value section)
A distributed hang has **no traceback**; every rank sits in a collective waiting for a peer that will
(Distinct from a **single-process** vanish; for OOM/reboot/SSH-HUP/kill, see `gotchas_universal.md`
U3; for the *inter-node* causes; fabric-manager, wrong NIC, MTU, the configured process-group timeout that *masks*
the real failure; see `references/multinode.md` MN1-MN4.)
### D19: The desync-debug toolkit: turn a silent freeze into a named mismatch
**Root cause**: a collective desync; ranks enqueued *different* collectives, or one rank never reached
**Fix; set these and relaunch the hang**:
- `export TORCH_DISTRIBUTED_DEBUG=DETAIL` + `export TORCH_CPP_LOG_LEVEL=INFO` → on mismatch PyTorch prints `Detected mismatch between collectives on ranks`, naming the op + sequence number per rank ([PyTorch forum](https://discuss.pytorch.org/t/torch-distributed-collectives-call-logging/172726)). (DETAIL itself does collectives; use to *diagnose*, remove for production; it can perturb timing.)
- **Flight Recorder** (`TORCH_NCCL_TRACE_BUFFER_SIZE=2000`) records the last N collectives per rank; configure the installed version's dump-on-timeout and stack-trace options when needed; read it to see which rank's queue is one collective behind.
### D20: One rank diverged (NaN/OOM) and the survivors hang waiting for it
The crash is the cause, the hang is the symptom; and without async error handling (D19) it surfaces
*diverged rank*, not the hang; common roots: one shard hit a bad sample (rank-dependent data), a
### D21: A rank-conditional collective (the `if rank == 0:` deadlock)
**Symptom**: hangs reproducibly at the *same* spot; often validation, logging, or checkpoint save.
### D22: Dataloader length mismatch across ranks (and the `set_epoch` shuffle bug)
genuinely uneven data use **Join** (D9). The shuffle-staleness is a correctness bug -
### D23: `print` / `tqdm` / eval / `torch.save` interleaving looks like a hang (but isn't always)
**Root cause**: by default **every rank executes everything**; 8× the prints, 8× eval, 8 ranks racing
**Fix**: gate pure side effects (logging, progress bar, file writes) to `if rank == 0:`; but keep any
## Pointers: handled elsewhere, do not restate