  Any Agent-Skills (SKILL.md)-compatible agent; Claude Code, Codex, Cursor, Trae, Gemini CLI, etc.
  durable-monitoring recipes assume a host background-task runner + scheduler; map to the running
# remote-gpu-trainer: Remote GPU Job Orchestration
short-term tenant on someone else's machine**; so the job is to *detach the work, make the result
or remote instance they do not own**; training, eval, ablation sweeps, batch inference, or large data
processing; on AutoDL, RunPod, vast.ai, Lambda, Paperspace, Chinese platforms (恒源云/矩池云/Featurize/
## When NOT to use: and what to use instead
| Managed multi-cloud price-shopping + auto spot-recovery across **Western** clouds | **SkyPilot** (has its own Agent Skill); then come back here to make your *code* resume-correct so its recovery actually works |
## Operating principles (the WHY: 10 invariants)
1. **Minimize paid wall-clock.** The meter runs the whole time; smoke locally on CPU before renting, launch detached, release the instant verification passes.
3. **Trust artifacts you loaded, not log lines that claim success.** "synced/saved/done" lies under a silently-failed write; a watcher's own state is also a claim; reconcile it against the real process/artifact.
4. **Know what survives stop vs destroy.** Per platform, identify exactly which mount survives a *stop* and which survives a *terminate*; the data you need often lives on the volatile one. (The single biggest portability trap.)
7. **Design for retry; failure is probabilistic, transfers are flaky, mirrors are route-specific.** Make wrappers idempotent + resumable; retry the *identical* config; wrap bulk transfers in `timeout`+resume loops; a mirror/proxy speeds ONE route; validate on the same route the real transfer uses.
10. **Teach the user the platform, don't just drive it.** Most users don't know a platform's non-obvious **conveniences** (one-click SSH-key registration, GPU-availability notifications, built-in panels) or its **danger clocks** (auto-release/auto-delete timers on a *stopped* box; AutoDL releases a 关机 instance after 15 days → data disk gone; a stop that keeps billing; low-balance purge). Surface them on first contact; #9 stops the agent *doing* the dangerous thing, #10 *warns the human* before the clock fires. Per-platform list → each profile's **Surface to the user** block.
Two rules govern the launch/wrapper/training code this skill has you write; corollaries of #1 and #8, not new invariants:
1. **Reuse before writing.** Take the lowest rung that already works before adding code: the base image's pre-installed stack + platform features → a framework/library utility (`torchrun` / `accelerate` / HF) → your existing `scripts/` templates → minimal new code. On a metered box a needless `pip install` also burns paid wall-clock and can break the image's ABI; Phase 1's rule (*the prebuilt image **is** the env; don't `conda create` on a rental*) is exactly this principle applied to dependencies.
2. **Floor; `minimum` bounds scope, not correctness.** Shrinking code must never drop what makes an expensive run survivable: checkpoint-to-durable + idempotent resume (#8), atomic writes, the error handling that prevents losing a long run, or seed/determinism logging. Keep one minimal self-check for non-trivial logic.
Read the matching profile **before Phase 0**; it owns every path, proxy, credential location, billing
| AutoDL (deepest, battle-tested) | `profiles/autodl.md` | ssh-rental | tmux | 关机 (stops meter, **keeps disk**; the AutoDL exception) |
> yet independently live-tested; lean on the Phase-0 live measurements and **re-verify any teardown/
**Phase 0; Environment audit.** Read the profile's STORAGE survival-matrix + region/DC-lock. Measure live:
**Phase 1; SSH + credentials.** Set the alias/env per the profile (the prebuilt image/base IS the env -
**Phase 2; Wrapper + CPU-smoke gate.** Build an idempotent `run_one`/`run_queue` from `scripts/` (parameterized
from the profile's OVERRIDES; **size batch/workers to the box for a standalone run, but PIN them across cells for a fair comparison**; `references/training/throughput-profiling.md`). **Run the cheap CPU smoke locally BEFORE renting**; it kills the dumb,
expensive failures (e.g. `python -m <your.train.module> --limit-batches 2 --epochs 1`; substitute your own entrypoint; this gate needs your training code plugged in). → **verify:** that smoke exits 0 on 2 batches with the logger disabled.
**Phase 3; Detached launch.** Launch via the profile's detach primitive; probe briefly (log head + alive +
no traceback), then **hand back**; never a blocking foreground `sleep`. → **verify:** within 60 s, the detach
**Phase 4; Durable monitoring.** For anything over ~1–2 h, deploy the **four-layer architecture**
**Phase 5; Aggregate + verify + teardown.** Checked-sync to durable storage (gate the success line on the
copy result; principle #3), then **load-and-verify each artifact** (`scripts/verify_local.py`), THEN the profile's
> **Iron Law; teardown gate:** NO `release` / `terminate` / `destroy` / file-delete until checkpoints are
> **irreversible** (deletes the disk); confirmation matters more, not less.
## Quick reference: the four facts that bite per platform
## Common gotchas (top 8 inline: full catalog in references/)
1. **SSH drops on `pkill -9`** (exit 255 + "Connection reset"); normal; re-ssh to verify, don't panic.
2. **tmux holds the script in memory**; editing it mid-run re-executes blocks; version the filename.
3. **Disk-full crashes `torch.save`** (`iostream error`); pre-budget; auto-prune `latest.pth`, keep `best`.
4. **cgroup OOM with no traceback** (bare `Killed` / exit 137); `num_workers × big-tensor`; size workers vs `memory.max`, not CPU count.
5. **Silent sync failure**; `cp … 2>/dev/null; echo synced` lies on a full/inode-exhausted FS; gate the success line on the actual copy result.
6. **Spot preemption grace is tiny (~5 s → ~0 s on the platforms profiled here; AWS-style 2-min grace only on clouds not profiled)**; a SIGTERM-flush handler is NOT a safety net; checkpoint on a timer to durable storage, load-latest unconditionally (`references/spot-resilience.md`).
7. **"Stop" rarely stops the meter**; only `terminate`/`destroy` does, and it's irreversible (deletes the disk). Know the verb from the profile before you click, and on RunPod a stopped Pod can even restart with zero GPUs.
8. **CRLF breaks `.sh` on Linux**; author on Windows → `.gitattributes` `*.sh text eol=lf`; on-box unblock `sed -i 's/\r$//'`.
Platform ops is only half the job; once the box is running, training breaks in its own ways. The
"make it run, fast, and not crash"; `verifying-dl-experiments` owns "is the *number* real"** -
- `references/training/oom-memory.md`; CUDA/VRAM + host-RAM OOM and the fit-it ladder (grad-accum → bf16 → activation-checkpointing → `expandable_segments` → FSDP/ZeRO → CPU/NVMe offload → LoRA/QLoRA); OOM-at-a-specific-step (first backward / val / longest batch); the memory snapshot + visualizer.
- `references/training/distributed-launch.md`; `torchrun`/`accelerate`/`deepspeed` launch + env contract, DDP/FSDP/ZeRO config, and the multi-GPU **HANGS** toolkit (one-rank-diverged, rank-conditional collective, dataloader-length mismatch). Multi-node wire → `references/multinode.md`.
- `references/training/precision-stability.md`; fp16/bf16/tf32 + AMP/GradScaler, NaN/Inf hunting (`detect_anomaly`), LLM **loss spikes** + divergence (warmup, clip, init, z-loss).
- `references/training/throughput-profiling.md`; GPU-bound vs data-bound vs comms-bound; dataloader knobs; `torch.compile` traps; flash-attention; `torch.profiler` / Nsight.
- `references/training/checkpoint-resume.md`; full-state save/resume mechanics, sharded (FSDP/DeepSpeed) checkpoints, and the resume bugs (epoch restart, data reshuffle, scaler/EMA dropped). Spot cadence → `references/spot-resilience.md`.
- `references/training/by-domain.md`; per-domain gotchas: LLM/transformer, vision (det/seg), diffusion, RL, multimodal/VLM.
- `references/training/convergence-debugging.md`; the **"runs but won't learn / learns badly"** layer: the overfit-one-batch smoke, params-not-updating, optimizer/LR/weight-decay/schedule config, loss-function footguns (double-softmax, BCEWithLogits, CE-target form), fine-tuning/freezing (frozen-BN drift, discriminative LR, LoRA wiring), and the training-dynamics dashboard (update:weight ratio, dead-ReLU, GradScaler-scale).
- `references/training/data-pipeline.md`; dataloader/dataset **correctness** (not speed): the worker-RNG augmentation-duplication bug, IterableDataset worker/rank sharding, collate/`__len__`/`pin_memory`/`spawn` contracts, and preprocessing/label/shuffle traps (RGB-vs-BGR, ToTensor ÷255, `set_epoch`).
These are **separate** Agent Skills, not bundled here; install them for the full experience. On an
- **`verifying-dl-experiments`**; owns *is-the-number-real*: smoke content, retry-vs-safeguard, keepable-checkpoint, eval sizing, tracker forensics, GPU-0%-util diagnosis. This skill owns *where/when/how-much-$*.
- **`huggingface-skills:hf-cli`**; the transport verbs (`hf download --resume`, `hf upload-large-folder`, `hf cache verify`); this skill owns the China-mirror swap + stall-retry (`references/china-network.md`).
- **`huggingface-skills:huggingface-trackio`**; hosted tracker so metrics survive teardown (gotcha U20); poll `trackio` alerts as a structured monitor instead of brittle ssh-tail.
- **`superpowers:verification-before-completion`**; the Iron Law's general form; gates every "training done / synced / teardown complete" claim.
- **`superpowers:dispatching-parallel-agents`**; independence predicate + reconciliation for ablation fan-out.
This skill is static, but every run can teach it something; without corrupting it.
not a gotcha; principle #3); **route it**; user/project-specific → the host's memory system,
`references/training/` (and offer an upstream PR); **never silently rewrite a skill file; draft the
<month>` stamp; re-verify any teardown/billing fact against current docs before betting money or data.
- `references/principles.md`; the 10 invariants expanded, with the cross-platform nuance behind each.
- `references/lifecycle_checklist.md`; the 6-phase runbook as a per-platform checklist.
- `references/gotchas_universal.md`; universal + mixed gotchas (TOC + grep index at top).
- `references/monitoring_patterns.md`; the four-layer durable-monitoring architecture + robust ssh-poll template.
- `references/ssh_transport.md`; ssh config, rsync/scp resumable patterns, secrets-via-stdin, CRLF, two-SSH-flavor caveat.
- `references/china-network.md`; mirrors table + HF_ENDPOINT + resumable-download ladder + the `no_proxy` trap (all CN platforms).
- `references/spot-resilience.md`; preemption signals, Young/Daly checkpoint cadence, atomic-write resume.
- `references/parallel_ablation.md`; FS-shared fan-out + the independence predicate + reconciliation.
- `references/multinode.md`; (advanced) NCCL / fabric-manager / elastic-training gotchas; single-box users skip.
- `references/training/`; the **DL-training debug layer** (8 files: oom-memory, distributed-launch, precision-stability, throughput-profiling, checkpoint-resume, by-domain, convergence-debugging, data-pipeline); see "When training breaks" above.
- `references/self-improvement.md`; the feedback loop: capture a new gotcha (at a bar) into memory or the catalog, personalize on first run, keep platform facts fresh.
- `scripts/`; wrapper templates (`run_one`/`run_queue`), monitors (`mem_monitor`, `gpu_health`, `reap_vram_zombies`), the read-only patrol (`health_patrol.sh.template`), transfer/aggregation (`download_loop`, `aggregate_to_fs`, `setup-china-mirrors`), the load-and-verify checker (`verify_local.py`), and the `verified`-stamp freshness linter (`check_staleness.py`).
- `profiles/<platform>.md`; the per-platform substrate (one per platform; `_schema.md` defines the 8 fields).
- `examples/autodl_sweep/`; one complete, runnable worked case end to end.