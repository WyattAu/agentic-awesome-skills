# Correct checkpointing & idempotent resume: full state, atomic write, sharded checkpoints, framework APIs
Make a training job resume from its last verified checkpoint after interruption; not "reload the weights and
quietly lost progress. **verifying-dl-experiments** (**REQUIRED**) owns *is the resumed number correct* -
`references/spot-resilience.md` (**REQUIRED** for any interruptible/spot tier); this file is the *content
- **The contract**; C1 full-state-list · C2 atomic-write · C3 load-latest-unconditionally · C4 durable-location
- **Sharded checkpoints (multi-GPU)**; C5 FSDP-FULL_STATE_DICT-rank0-OOM · C6 FSDP-SHARDED_STATE_DICT · C7 DCP-(dcp.save/load) · C8 DeepSpeed-ZeRO-dir+zero_to_fp32
- **Framework APIs**; C9 HF-Trainer-resume_from_checkpoint+save_total_limit · C10 Accelerate-save_state/load_state · C11 Lightning-ModelCheckpoint+ckpt_path
- **The resume BUGS**; C12 epoch-restarts · C13 data-reshuffles/order · C14 LR-schedule-resets · C15 scaler-not-restored · C16 EMA-not-saved · C17 save_total_limit-deletes-best · C18 strict-load-key-mismatch
- **Pointers**; disk-full on save → gotchas_universal.md U6 · silent sync → U33 · keepable-policy/save_top_k → verifying-dl-experiments (skill) · cadence/Young-Daly → spot-resilience.md
### C1: A checkpoint that restores only weights is NOT a resume: save the FULL training state
total epochs than an uninterrupted run; because the resume silently restarted the epoch, reset the
| optimizer `state_dict` | Adam m/v momentum; losing it = a cold optimizer restart (C12) |
| LR-scheduler `state_dict` | step-based LR position; losing it resets the schedule (C14) |
| AMP `GradScaler` `state_dict` | the loss-scale + growth tracker; losing it triggers an inf-scale stall (C15) |
| EMA / SWA shadow weights (if used) | the EMA copy is often what's evaluated; losing it = eval on the wrong weights (C16) |
The illustrative atomic skeleton (not a complete production state implementation) that assembles part of this dict is in `references/spot-resilience.md` §5; do not
### C2: Write atomically: tmp → fsync → os.replace (a kill mid-write corrupts a naive save)
**Root cause**: overwriting `latest.pth` in place is **not** atomic; a kill partway through leaves a
### C3: Load-latest UNCONDITIONALLY on startup → idempotent resume
**Fix**: one code path that loads the latest verified compatible checkpoint for this exact run/config if it exists, else starts fresh; so the
### C4: Checkpoint to the platform's DURABLE location, not local scratch
**Symptom**: resume after a managed-spot replacement (or a `terminate`/`destroy`) finds no checkpoint -
is gone (principle #4; know what survives stop vs destroy).
disk survives; see each profile's STORAGE survival-matrix and the SKILL Quick-reference table. Gate the
### C5: FSDP `FULL_STATE_DICT` OOMs on rank 0 when gathering a large model
**Fix**: when taking a full (consolidated) dict, offload it to CPU and build it on rank 0 only -
### C6: `SHARDED_STATE_DICT`: each rank saves its own shard (no gather, no rank-0 OOM)
**Fix**: use `StateDictType.SHARDED_STATE_DICT`; every rank writes only its own shard, so there is no
GPU count). Tradeoff: a sharded checkpoint is a *directory of N files*, not a single `.pth`; convert to a
### C7: Distributed Checkpoint (DCP): `dcp.save` / `dcp.load` for FSDP/sharded models
`dcp.save(state_dict, checkpoint_id=DIR)`; it writes **≥1 file per rank in parallel** and auto-manages FQN
### C8: DeepSpeed ZeRO: a checkpoint *directory* per save + `zero_to_fp32.py` to consolidate
ranks; the on-disk checkpoint is inherently sharded across per-rank files; it is not a single fp32 model.
- **Save/resume training**; `model_engine.save_checkpoint(save_dir, tag)` /
- **Export a single fp32 model**; DeepSpeed auto-drops a `zero_to_fp32.py` into the checkpoint dir; run
### C9: HF Trainer: `resume_from_checkpoint` + `save_total_limit` (and what it actually saves)
AMP `scaler.pt`; the full state. Without `resume_from_checkpoint` the run starts cold.
`checkpoint-*` and **deletes older ones in `output_dir`**; the built-in disk-budget knob (pairs with
### C10: Accelerate: `accelerator.save_state(dir)` / `load_state(dir)` + dataloader skip
Prepared Accelerate schedulers are normally tracked automatically. Custom stateful objects not handled by prepare can be registered explicitly -
`accelerator.register_for_checkpointing(my_scheduler)`; then verify the checkpoint actually includes it (C14). For
### C11: Lightning: `ModelCheckpoint` + `trainer.fit(ckpt_path=...)` (don't use `resume_from_checkpoint`)
`.ckpt` is a full dump; epoch, global step, LightningModule `state_dict`, **all** optimizer + LR-scheduler
  callback. Lightning's DeepSpeed strategy writes a ZeRO dir; convert with
These are the "it ran without error but the result is wrong" traps; confirm the fix with the
### C12: Epoch/step restarts from 0 despite "resuming"
LR warm-up replays. (The remote-ops version of this; a tmux script re-executed mid-run; is
### C13: Data reshuffles / repeats the same order after resume
unless `sampler.set_epoch(epoch)` is called each epoch; so every epoch (and every resume) produces the
### C14: LR schedule resets (cosine restarts, warm-up replays)
step-based scheduler advanced *per optimizer step* must restore the **step**, not the epoch; restoring
### C15: AMP `GradScaler` not restored → "No inf checks were recorded" / scale stall
**Root cause**: the `GradScaler` holds dynamic state; `scale`, `growth_factor`, `backoff_factor`,
`growth_interval`, `_growth_tracker`; that evolves during training; dropping it resets the scaler. The specific no-inf-checks assertion often means no gradients reached the optimizer or an invalid scaler/step sequence; it is not proof of missing checkpoint state
(`save_state`), and Lightning (16-bit factor) all do this automatically; the bug bites hand-written loops.
### C16: EMA / SWA shadow weights not saved → eval on the wrong weights after resume
over many steps; because the EMA copy restarted from the raw weights.
This is a *which-weights-are-correct* concern at the boundary; cross-link **verifying-dl-experiments**
### C17: `save_total_limit` / `save_top_k` deletes the very checkpoint resume needs
and neither guarantees the most-recent-step checkpoint is the one kept; so the resume anchor can be the
### C18: `load_state_dict` key mismatch on resume (`module.` prefix, compiled-model prefix)
**Fix**: save the **unwrapped** module; `model.module.state_dict()` (DDP) /
`accelerator.unwrap_model(model).state_dict()` / `model._orig_mod.state_dict()` (compiled); so the
## Pointers: owned elsewhere, do NOT restate here
- **Cadence; when/how often** (Young/Daly `W = sqrt(2·mu·C)`, grace windows, opportunistic SIGTERM