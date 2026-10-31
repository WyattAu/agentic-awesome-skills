# Universal & mixed gotcha catalog: every metered remote-GPU rental
The cross-platform gotchas: they bite on **any** metered, isolated, rented GPU; only the concrete
value* in the fix; the rule stays here, the value lives in a profile. Platform-only gotchas (AutoDL's
TB-pin, the wandb-key classifier, the network_turbo proxy literal) do NOT live here; see each profile's
- **Process & SSH**; U1 SSH-dies-on-kill · U2 tmux-holds-script-in-memory · U3 vanished-process-4-causes · U4 kill-drops-SSH-before-relaunch · U5 hook-safe-launch
- **Disk & Storage**; U6 disk-full-crashes-torch.save · U7 storage-fails-on-inodes · U8 stage-hot-data-to-NVMe
- **Memory & OOM**; U9 cgroup-OOM-num_workers×tensor · U10 VRAM-OOM-vs-cgroup-OOM · U11 zombie-VRAM-nvidia-smi-cant-see · U41 host-metrics-lie/oom_kill-counter
- **Transfer & Download**; U12 scp-resets→resumable-loop · U13 scp-into-uncreated-dir · U14 egress-surcharge+same-AZ · U15 compress-before-the-wire
- **Monitoring**; U16 stale-waiters/zombie-monitors · U17 unquoted-pipe-grep-hang+robust-poll · U18 two-leg-remote-self-completion · U19 tracker-deletion-lags · U20 hosted-tracker-survives-teardown · U39 live-panel/TB-silently-empty (path/port/process mismatch) · U43 block-buffered-stdout-looks-frozen
- **GPU health**; U21 nvidia-smi-util%-is-a-liar · U22 Xid-48/79-dead-GPU-re-rent · U23 thermal/power-throttle-steals-25-40%
- **Dataloader & IO**; U24 dataloader-starvation-knobs · U25 many-small-files→shard-into-tar · U40 intra-op-thread-oversubscription-starves-GPU
- **Env & Container**; U26 CRLF-breaks-sh · U27 overlay-config-files · U28 CUDA-toolkit-vs-driver-vs-torch · U29 install-from-lockfile · U30 pin-image-by-sha256 · U31 container-runs-but-no-GPU · U42 box-code-drift/verify-deploy
- **Cost & teardown**; U32 task-epoch-default · U33 silent-checked-sync
- **Secrets & trackers**; U34 secrets-via-stdin · U35 tracker-offline-without-key
- **Delegated (cross-link only)**; U36 cuDNN-nondeterminism · U37 matplotlib-2^16 · U38 GPU-0%-util-data-bound
- **Pointers**; spot/preemption → `references/spot-resilience.md`; multi-node/NCCL → `references/multinode.md`
### U1: SSH disconnects on `pkill -9` (exit 255, "Connection reset")
### U2: tmux holds the script in memory; editing it mid-run re-executes blocks
IS re-read fresh for the *next* item; but only if none is parked mid-script. (principle #6.)
**Fix**: **never overwrite a script any process is executing**; check `pgrep -af <script>` first; version
**Related detach trap; a non-exported var doesn't cross into the detach primitive.** A `VAR=x` set in
**exported** (or inlined in the launched command); the job sees it empty, and a launcher/monitor that
### U3: A vanished remote process ≠ OOM: enumerate the 4 causes
**Root cause is one of four**; OOM is only one:
1. **Machine restart / reboot**; `dmesg` is *clean*, GPU idle, cgroup roomy, `uptime` low. Most-missed: nothing in the log hints at it.
2. **OOM-kill (`-9`)**; `dmesg | grep -i 'killed process'` shows it, memory was tight (U9).
3. **SSH HUP**; a foreground (non-`nohup`/`tmux`/`setsid`) launch dies when its parent SSH drops.
4. **Manual kill**; an earlier `pkill` matched more than intended.
**Fix; diagnose cheap → conclusive before "fixing"**:
"fix" a phantom OOM; that masks the one variable under test. **Separate trap**: a dropped poll connection
≠ the training dying; re-ssh and check the process/artifact directly (`pgrep -af train`, log tail,
### U4: `kill` drops the SSH before a relaunch in the SAME command runs
**Fix**: split; kill in one ssh call, relaunch (with NO kill) in the next. To stop a kill/poll pattern
### U5: Hook-safe remote launch: keep env activation VISIBLE in the launch command
**Root cause**: the hook scans the **command string**; it cannot see inside an scp'd script, and a bare
`ssh <host> 'source /path/to/conda.sh; conda activate <env>; nohup bash /root/job.sh ...'`; the script
### U6: Disk-full crashes `torch.save` with `iostream error`
**Fix; prevent**: budget current checkpoint, temporary replacement, full-state latest, best and tracker cache across concurrent runs. Copy to verified durable storage before pruning only redundant periodic checkpoints. Preserve a full-state latest resume checkpoint; best weights alone may not resume optimizer/scheduler progress. Recover space using identified redundant files, never blindly deleting every `*.tmp` or latest checkpoint.
### U7: Storage fails on the dimension (and location) not being watched
**Symptom**: `cp`/`mkdir` fails `No space left on device`, yet `df -h` shows ~34% used; because `df -i`
**Root cause**: disk dies on **inodes before bytes**; the classic trigger is **per-sample eval output**,
`du`, not assumptions** (`du -sh ~/.cache/huggingface/hub/models--* | sort -rh`). Clean by **value**; keep
### U8: Stage hot data to local NVMe before training
throughput than instance-local NVMe; HDD-vs-NVMe gaps reach ~35×.
### U9: `num_workers` × a big in-RAM tensor → cgroup OOM-kill (bare "Killed", exit 137)
Shrinking the problem also fixes it; a smaller matrix dim shrinks footprint *quadratically* (dim 1024 ≈
### U10: VRAM OOM (a big model or a concurrent job) is distinct from cgroup-RAM OOM (U9)
**Root cause**: **VRAM**; the sum of concurrent jobs' allocations plus fragmentation exceeds the card. NOT
fragmentation. (Run heavy DL on the box; do static/shape checks locally; cross-link
### U11: A zombie holds VRAM `nvidia-smi` cannot see → OOM on an "empty" GPU
### U41: On a shared box, `uptime`/`free` describe the whole physical host, not your container: use cgroup-scoped readings + the `oom_kill` counter
**Symptom**: a detached run looks "dead" or "the host is overloaded"; `uptime` shows load average 400+,
`top`/`free -m` look maxed; so you suspect contention or an OOM-kill. But the job's own checkpoint `mtime`
- memory; `/sys/fs/cgroup/memory.current` vs `memory.max` (not `free -m`);
- were YOU OOM-killed; the **`oom_kill` counter** in `/sys/fs/cgroup/memory.events`
- CPU pressure; `/sys/fs/cgroup/cpu.stat` / `cpu.pressure`.
A high host load with your cgroup roomy and `oom_kill 0` is a **noisy neighbor**, not your bug; don't
### U12: `scp -r` of a large dir resets mid-transfer → per-dir resumable loop
**Fix**: loop **per-dir**, each its own SSH session; one failure doesn't lose the others, and re-running
### U13: `scp` into a remote dir a sibling command was supposed to create (race)
- the foreground command that would have `mkdir`-ed `/root/x` ran later, or was blocked/cancelled.
### U14: Egress and cross-zone traffic can add material cost
### U15: Compress before the wire
**Fix**: zstd/gzip the payload before transfer; benchmark compression on representative data; already-compressed data may not shrink. Do not quantize a resumable checkpoint or change scientific precision merely to transfer it faster. Compounds with U14 (less egress $) and U12 (fewer bytes to
### U16: Stale background waiters pile up; supersede a run → STOP its waiter; pick the right lifetime
waiter but never stopped the OLD one; its marker (in a superseded log) never appears, so it loops forever.
**Fix**: one waiter per live run; superseding a run, stop the old waiter first (`TaskStop`; cross-session
IDs aren't stoppable from a resumed session; dismiss those from the UI). Multi-hour wait → a **persistent
### U17: A silent background monitor that never returns: usually an unquoted `|` in grep
"alive." The run looks hung; but the actual job finished and wrote results fine.
**unquoted `|` in grep**; `grep -hE noise-sweep|snr=|wrote log`; the shell splits it into THREE piped
notification. (Background tasks notify on EXIT only; no 600 s cap; foreground Bash is the capped one, U16.)
**Fix; robust remote-poll template**:
- **Bound the ssh**: `ssh -o ConnectTimeout=15 -o ServerAliveInterval=10 -o ServerAliveCountMax=3 …`; a blip self-kills in ~30 s instead of half-open hanging for minutes.
### U18: "I'll check periodically" is a lie unless a trigger is armed; two-leg remote self-completion
**Symptom**: a promise to monitor a multi-hour remote run, then no report for a day; because between turns
marker then runs eval) sequences stages while the host remains healthy but gives no *reporting cadence*; nothing re-invokes the
**Fix; the two-leg pattern**:
- **Remote self-completion (survives session/SSH death, not host failure)**: chain `train → eval → touch marker` under one `nohup ... </dev/null >log 2>&1 &`. Detect "done" by a **log marker** (`grep -q 'QUEUE DONE' master.log`), NEVER by `pgrep`; the waiter's own command line contains the pattern, so `pgrep -f` matches itself and loops forever (U17).
- **Live progress (best-effort)**: a session-bound local loop (e.g. `/loop 30m` / cron `3,33 * * * *`) that ssh-polls with the *local* key. Be honest it dies when the session closes; the remote still finishes; the user re-pings to pull.
- **Don't promise autonomous cross-session polling you can't deliver.** (`tmux` is often absent on a fresh box and `apt-get install` fails offline; `nohup ... </dev/null >log 2>&1 &` is zero-dependency and survives SSH drop; verify with `pgrep -af <script>`.) Full architecture → `references/monitoring_patterns.md`.
### U19: Tracker run deletion lags; a fresh export resurrects "deleted" runs
### U20: Local logs die with the instance: use a hosted tracker
**Symptom**: TensorBoard event files written to an ephemeral box vanish on teardown; every curve gone after
**Fix**: with approval for the data transfer and visibility, log metrics to a **hosted tracker** so they survive teardown; `trackio.init(space_id=...)` or
### U43: A detached run's log looks frozen for minutes though training is fine: stdout is block-buffered off a TTY
"hung / died" and the reflex is to kill it; but checkpoint `mtime`, TB scalars, and `nvidia-smi` all show
it is a pipe or file**; exactly the detached case. The log only flushes when the buffer fills, so a
**Fix**: run unbuffered; `python -u` or `PYTHONUNBUFFERED=1` (the shipped `scripts/run_one.sh.template`
cadence**; checkpoint `mtime`, the TB scalar API, `nvidia-smi` (monitoring_patterns §0 corollary; the
### U21: `nvidia-smi` GPU-Util % is a liar
**Fix**: correlate util with **SM clock** (`clocks.current.sm`), memory-bandwidth util, and power draw -
### U22: Xid 48/79 requires hardware/driver investigation
**Fix**: on a *rental* the card can't be reseated; preserve results and report the exact fault to the provider; propose an approved replacement only after diagnosing the recovery options; don't
### U23: Thermal/power throttling silently steals 25–40% with no error
**Symptom**: "the same code is slower than yesterday"; no error, no crash, just lower throughput.
**Fix**: detect; SM clock falling below base while temp >83 °C, or
### U24: GPU starves at 10–70% waiting on the dataloader, not on compute
**Root cause**: the input pipeline can't keep the GPU fed; too few workers, no prefetch, host↔device copies
**Fix; tune in order**: benchmark a small `num_workers` sweep within cgroup CPU and memory budgets (sized against per-worker footprint, U9),
### U25: Millions of small files on a network/object store → transaction-overhead death; shard into tar
and the per-sample-vis trap; cap and shard rather than emitting a file per sample. Pairs with U8 (stage the
### U40: A vCPU-sliced rental starves its own GPU: torch intra-op threads default to the HOST core count, not your cgroup quota
their intra-op thread pools to the **physical** core count; `torch.get_num_threads()` / `OMP_NUM_THREADS`
that launches kernels and feeds the GPU can't keep up and the card idles. No OOM, no error; pure scheduler
**Fix**: cap the pools to your **slice's** vCPU count before launch -
### U26: CRLF breaks `.sh` on Linux (authored on Windows)
option`, `cd: /path\r: No such file or directory`, `syntax error near unexpected token $'do\r'`; every
### U27: `-o dotted.key=value` overrides explode on null parents → freeze protocols as overlay config FILES
### U28: The CUDA-toolkit ↔ host-driver ↔ torch-build triangle
**Root cause**: three independently-versioned layers must agree; **the host driver is host-global and a
### U29: "Same version, different result": top-level pins let transitive deps drift → install from a lockfile
### U30: A Dockerfile is NOT reproducible: pin the base image by `@sha256:` digest
**Fix**: pin the base image by content digest; `FROM image@sha256:<digest>`, not `:latest`; to pin the base layers. Also lock dependencies and build inputs; a base digest alone does not make the whole build bit-reproducible. (`pin_image_by_sha256` is a per-platform expectation where the image is the env
### U31: Container runs but trains 100× slower = the GPU was never attached (CPU-only)
**Root cause**: the container has no GPU; launched without `--gpus all`, or the NVIDIA Container Toolkit is
container before training**; never assume GPU attachment from a clean `docker run`.
### U42: The box runs a hand-synced copy with no git remote; a fix you "committed" may not be deployed: verify it is ON the box before trusting a run or tearing down
**Root cause**: most rentals have **no git remote**; the box holds a working tree you pushed by
**Fix**: treat code deploy like the checked-sync (**U33**); **verify, don't assume**. After syncing, grep
depends on the fix, and part of the **Phase-5 teardown gate**; a verdict produced by stale code is not the
### U32: A task's default epochs differ from another task's; CLI `--epochs` silently overrides the right value
config-drift instance; really a smoke/sanity target (cross-link verifying-dl-experiments **REQUIRED**).
### U33: Silent sync failure: gate the success line on the actual copy result
**unconditionally**; it never checks the exit code. When the durable FS is inode-exhausted (U7) `mkdir`
**Fix; checked, gated sync**:
    echo "[$(date +%H:%M:%S)] !! SYNC FAILED for $NAME (check df -i); data disk is still source-of-truth"
### U34: Move credentials to the box without the secret ever appearing in a command
### U35: `WANDB_MODE=offline` still dies without an API key in wrapper stacks → zero curves
first log lines show `Syncing run <name>` + a run URL; treat the *absence* of that line as a failure. Run
## Delegated: cross-link only, do NOT restate here
### U36: cuDNN nondeterminism
### U37: matplotlib `2^16`-per-axis limit on large eval visualization
### U38: GPU at 0% util but training IS running (CPU-data-bound, not stalled)
`nvidia-smi` reads ~0% util yet the step log advances and model memory is loaded; a heavy per-sample CPU
### U39: Live monitoring shows nothing (TensorBoard panel empty / `INACTIVE`) but training is fine
goes silent; yet the run is healthy: the loss advances on the box and the event/log files exist. You
failure. (1) **Path mismatch**; the platform's built-in panel reads a FIXED logdir/port and your logger
`SummaryWriter(log_dir="runs/<exp>")` is invisible to it). (2) **Process died / never backgrounded**; the
or on session/SSH drop, so nothing serves the curves. (3) **Port not exposed**; the service is up on the
**Fix** (the rule is universal; the *value* is per-profile): (1) **align the path**; point your logger at
no retrain; the running writer keeps appending and the panel reloads it. The pinned path lives in the
§7). (3) **expose the port the platform's way**; CN built-in tiles declare it at rent time (`china.md`),
## Pointers: gotchas catalogued elsewhere