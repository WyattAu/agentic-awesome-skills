# Lifecycle Checklist: the 6-phase runbook as a per-platform checklist
platform profile** (`profiles/<platform>.md`, 8-field schema in `profiles/_schema.md`); this file never
- Phase −1; one-time setup (skip if reused)
- Phase 0; environment audit
- Phase 1; SSH + credentials
- Phase 2; wrapper + CPU-smoke gate
- Phase 3; detached launch
- Phase 4; durable monitoring
- Phase 5; aggregate + verify + teardown
> phase already done. Universal gotchas referenced by id live in `references/gotchas_universal.md`; not
## Phase −1: One-time setup *(skip if reused from a past project)*
- [ ] **First contact; surface the profile's "Surface to the user" block** to the user: the platform's non-obvious **conveniences** (one-click SSH-key registration, GPU-availability notifications, built-in panels) and its **danger clocks** (auto-release/auto-delete timers, stop-still-bills, low-balance purge). Don't assume they know; the danger clocks cost data or money (principle #10).
- [ ] `~/.ssh/config` alias per instance, with keepalive (`ServerAliveInterval`/`ServerAliveCountMax`); see `references/ssh_transport.md`.
## Phase 0: Environment audit
- [ ] Measure live, do not assume: `df -h && df -i <profile data mount>` (inodes die before bytes; principle #5), cgroup `cat /sys/fs/cgroup/memory.max`, `nvidia-smi`.
- [ ] `du -sh` the real space hogs on the actual mount (symlinked caches hide; principle #5), not the dir assumed.
## Phase 1: SSH + credentials
- [ ] Set the SSH alias/env per the profile's NETWORK section; note the SSH flavor (a proxied/basic SSH may not `scp`/`rsync`; direct-TCP required; ports may change on restart).
- [ ] Use the prebuilt image/base AS the env; do NOT `conda create` on a rental (throwaway-instance exception: the image IS the env).
- [ ] Push secrets via **stdin, never onto a shared/durable FS** (a shared FS is multi-project); pattern in `references/ssh_transport.md`. Reference creds by env-var NAME / file path, never inline a key.
## Phase 2: Wrapper + CPU-smoke gate
- [ ] Build the per-cell queue/config files with one isolated write path per cell (no shared mutable output; parallel ablation needs this; `references/parallel_ablation.md`).
- [ ] **Run the cheap CPU smoke LOCALLY, BEFORE renting**; 1–2 batches, logger disabled, tiny shapes; it kills import/config/shape/scale bugs for ~free (principle #2). Smoke *content* → **verifying-dl-experiments** (REQUIRED).
## Phase 3: Detached launch
- [ ] Launch via the *profile detach primitive* (tmux / `sbatch` / k8s Job / commit); survives an SSH drop; confirm whether it also survives an instance restart (profile DAEMON section).
- [ ] Push code/data with a resumable transfer (`rsync --partial` or `timeout`+resume loop; principle #7); never edit a script under a live run; version filenames (principle #6).
## Phase 4: Durable monitoring
- [ ] Use `run_in_background` (no duration cap, notifies on exit; a Claude Code primitive; other hosts map per `monitoring_patterns.md` §7) for long waits; never foreground-poll. NEVER an unquoted `|` inside a poll-regex; it reads stdin and hangs forever.
- [ ] Reconcile each watcher against the job's REAL process/artifact (`tmux ls`/`squeue`/`pgrep` + output `mtime`); a watcher's own state is a claim, not ground truth (principle #3). Tear a watcher down when its job is superseded.
## Phase 5: Aggregate + verify + teardown
- [ ] Run the aggregation step (`scripts/aggregate_to_fs.sh`, idempotent; safe to re-run) to checked-sync results to the *profile durable mount*. **Gate the success line on the actual copy result**; `cp …; echo synced` lies on a full/inode-exhausted FS (principle #3, gotcha U33).
- [ ] Pull results to local (resumable per-dir scp/rsync loop); HARD-sanitize the local target to `/path/to/local`; never a real personal path.
- [ ] Record disclosable run facts for the paper: CLI overrides, tracker summary URL (a hosted tracker survives teardown; **huggingface-skills:huggingface-trackio**). Transport verbs (`hf download --resume`, `hf upload-large-folder`) → **huggingface-skills:hf-cli**.
> **Iron Law; teardown gate:** NO `stop` / `release` / `terminate` / `destroy` / file-delete until
> meter-stopping verb is **irreversible** (deletes the disk); confirmation matters *more*, not less. The
The verb that stops the meter, what each preserves, and irreversibility; bind the platform-specific verb
| **stop / 关机 (power-off)** | Sometimes; depends on profile (AutoDL: yes, keeps disk; RunPod/vast: still bills storage 1–2×) | Disk tier per profile survival-matrix | Yes; instance restartable |
| **release idle instance** | Yes | Only the durable/shared mount (data disk gone) | No; instance + container disk destroyed |
| **terminate** | Yes | Only a network/persistent volume, if one was mounted | **No; irreversible**, disk deleted |
| **destroy** | Yes | Nothing on the box | **No; irreversible**, total loss |
| **delete durable files (keep subscription)** | Storage trickle only | Subscription survives for new data | No; those files gone |
| **cancel durable storage subscription** | Storage cost only | Nothing | **No; irreversible**, all durable data lost |
Categorize before reacting; retry the **identical** config; hand-patching one run destroys comparability
- [ ] **Disk-full** (exit 1 + `iostream` / "No space left"): prune the *profile scratch* (`SCRATCH=`; periodic checkpoints, unused caches), keep `best`; if cleanup can't free enough, **ask to expand the disk**, never silently shrink the experiment (principle #9). Then retry.
- [ ] **Real bug** (CUDA OOM, code error, all-zero metric): stop, investigate code; do NOT retry blindly.