meter_stop_verb: manual       # nothing reclaims the box; a forgotten instance bills 24/7
# Profile: generic-SSH: the DEFAULT (bare box) + Slurm / Kubernetes / Colab-Kaggle diffs
channel and teardown is manual**; every other platform profile is a *diff* against this baseline.
> **Surface to the user up front (principle #10):** ⚠️ Danger clock; there is usually **no auto-release / idle timer to save you**: a forgotten box **bills 24/7** until you tear it down, and teardown is entirely manual (no platform safety net). Reality; you **expose ports yourself** (an `ssh -L` tunnel for TB/Jupyter); on Slurm a job dies at **walltime**; design the requeue.
**Universal gotchas are NOT restated here**; see `references/gotchas_universal.md`.
- THIN DIFF; SLURM (sbatch replaces tmux)
- THIN DIFF; KUBERNETES (a Job manifest replaces the shell)
- THIN DIFF; COLAB / KAGGLE (not SSH-orchestratable)
- **Entry point:** `ssh user@host`; key-based, fronted by an `~/.ssh/config` alias so the rest of
  the workflow says `ssh gpu-box`. There is **no platform API, console, or CLI**; SSH is the *only*
- **Push code:** `rsync -avz --partial ./proj/ gpu-box:~/proj/`; resumable, delta-only on re-syncs;
  cheaper pipe to HF/S3 than a home connection; pushing a 50 GB checkpoint over a residential uplink
- **Env contract:** whatever the host ships. There is no prebuilt "base" guarantee; inspect
  pin it. State the seed/determinism in the run itself; no platform does it here (**REQUIRED:**
## 2. STORAGE MODEL  *(the survival matrix: principle #4)*
box. Caps are host-dependent; do **not** carry over an AutoDL ~200K-inode or ~200 GB constant.
| Root / home disk | `/` , `~` | yes (box keeps running) | **no** (destroy deletes the box) | host-dependent; `df -h`/`df -i` |
| Attached block volume (if any) | `/path/to/mount` | yes | depends on provider; verify before destroy | host-dependent |
platform enforces**; the box runs until *manually* stopped, and a destroy wipes the disk with no
(§5). Disk fails on inodes before bytes and the real hog hides in a symlinked cache; audit the
  set the mirror manually; `export HF_ENDPOINT=https://hf-mirror.com` (or `HF_HUB_ENABLE_HF_TRANSFER=1`
  off-GFW); and validate the speed test on the **same route** the real transfer uses (principle #7).
- **SSH flavor:** direct-TCP key-based SSH; `scp`/`rsync` work normally (unlike the proxied SSH on
pattern → `references/spot-resilience.md`. (Spot-rented bare boxes exist; if the provider can evict,
no idle timer, no auto-release, no scheduler that ends the job. **A forgotten box bills 24/7**; an
  a phone call); and on most bare rentals it is **irreversible** (deletes the disk).
  (principle #3). It survives an SSH drop; it does **not** survive a box reboot; relaunch after one.
- **No native queue**; the operator IS the scheduler, monitor, and janitor. Use the parameterized
  being read (principle #6; version the filename).
- **GEN1; Forgotten box bills 24/7.** Symptom: a week-old invoice for an instance that finished
  (never auto-act; principle #9); for cross-session safety set a `/schedule` reminder to re-check.
- **GEN2; SSH drop kills the run (no tmux).** Symptom: training dies the moment the laptop sleeps or
  → Fix: launch inside `tmux` (or `nohup … & disown`) **before** the long run starts; not after it is
- **GEN3; `scp` restarts from zero on a reset; `rsync` does not.** Symptom: a 40 GB re-sync that
- **GEN4; CRLF breaks `.sh` on the Linux box.** Symptom: `bash: $'\r': command not found`, or a
- **GEN5; Heavy DL static-checked on the wrong machine.** Symptom: an OOM or a CUDA mismatch only
- **GEN6; A box reboot silently orphans the run (`tmux` does not survive it).** Symptom: a detached
  → Root cause: `tmux`/`nohup` survive an SSH drop but **not** a host reboot; the rental rebooted (host
- **GEN7; A second concurrent run silently halves throughput by oversubscribing the GPU.** Symptom: two
  → Root cause: a bare box has **no scheduler**; nothing prevents two processes sharing one GPU, so they
  contend for VRAM and SM time. → Fix: the operator *is* the scheduler; serialize with the
- **GEN8; Watching a poll connection, not the run, declares a false death.** Symptom: the ssh-poll
The box has no console; every diagnostic is an ssh one-liner. Run these *separately* (a kill drops the
- **Is the run alive or orphaned?** `ssh gpu-box 'tmux ls; pgrep -af <train-script> | head'`; empty
- **Why did it die (the 4-cause ladder)?** `ssh gpu-box 'dmesg 2>/dev/null | grep -iE "killed process|out of memory|Xid" | tail; uptime'`; OOM line ⇒ U9/U10; clean dmesg + low uptime ⇒ reboot (GEN6); `Xid 48/79` ⇒ dead GPU, re-rent (U22).
- **GPU health, not just util%:** `ssh gpu-box 'nvidia-smi dmon -s pucvmet -d 1 -c 5'`; read SM clock + power, not `GPU-Util` (a liar, U21); a holder `nvidia-smi` cannot see ⇒ `fuser -v /dev/nvidia*` (U11).
- **Disk before it bites:** `ssh gpu-box 'df -h <mount>; df -i <mount>'`; inodes hit 100% before bytes (U7); the byte-hog often hides in `~/.cache/huggingface` (`du -sh ~/.cache/huggingface/hub/models--* | sort -rh`).
- **Stuck download?** A transfer with a live process but a flat `df` is stalled, not progressing -
CRED_FILE=~/.netrc on the box's local disk, streamed in via stdin; never onto a shared/durable FS
DETACH=tmux            (the swappable plug; replaced by sbatch / Job / commit in the diffs below)
# THIN DIFF: SLURM  *(sbatch replaces tmux)*
`tmux+nohup` is **replaced** (not supplemented) by `sbatch`; a submitted batch job survives logout
  (status; replaces "reattach tmux") · `sacct -j <jobid>` (post-mortem: exit code, maxRSS, elapsed)
  · `scancel <jobid>` (kill). Logs go to `slurm-%j.out` (arrays: `slurm-%A_%a.out`); file-based, same
- **Walltime ceiling; the hard new constraint:** `#SBATCH --time=HH:MM:SS` and at the limit each task
  (the `B:` prefix signals the **batch shell**, not the steps; **Slurm may fire it up to 60 s EARLY** -
- **No per-hour teardown; watch fairshare.** Nodes are not `shutdown`; the job just ends. The
  (**Apptainer/Singularity**; Docker is usually banned).
  go; node-local **`$TMPDIR` is wiped when the job ends**; stage scratch to `$TMPDIR`, checkpoint to
- **SLURM1; Checkpoint *inside* the signal handler corrupts the checkpoint.** Symptom: `--requeue`
  load. → Root cause: a Python signal handler can fire **after any bytecode instruction**; including
  mid-backward-pass; so checkpointing directly in the handler races with training (verified
  github.com/Lightning-AI/pytorch-lightning#21406 2026-06). → Fix: the handler does the **minimum** -
- **SLURM2; Warning signal arrives too late; the SIGKILL lands mid-write.** Symptom: the
  slacks compound; Slurm may send the warning **up to 60 s early**, and at the actual wall the
- **SLURM3; `srun` inside `sbatch` no longer inherits `--cpus-per-task` (Slurm ≥ 22.05).** Symptom: a
  `--gpus-per-task`/`--gres` on the `srun` too; a step does not inherit the allocation's GRES by default.
- **SLURM4; OOM is a job STATE, not a Python traceback.** Symptom: the job dies with no error in the
  (dataloader workers × a big tensor), distinct from VRAM OOM (U10); **do not** shrink batch for a
- **SLURM5; `$TMPDIR` checkpoints evaporate when the job ends.** Symptom: a requeued/array job finds an
- **Still queued or running?** `squeue -u $USER -o '%i %T %r %M %l %R'`; the `%r` Reason column explains
 ; `State=TIMEOUT` ⇒ walltime kill (raise `--time` or requeue); `OUT_OF_MEMORY` ⇒ SLURM4; `PREEMPTED`/`NODE_FAIL`
  ⇒ requeue territory; `ExitCode` like `0:9` means killed by **signal 9** (SIGKILL; the KillWait expired).
 ; a mismatch ⇒ SLURM3 (`--gres`/`--gpus-per-task` not on the `srun`).
# THIN DIFF: KUBERNETES  *(a Job manifest replaces the shell)*
schedules a pod and a `Job` controller **replaces it on failure** up to `backoffLimit` (**default 6** -
2026-06). The "detach from my connection" problem vanishes; the pod never had a connection to the shell.
  `requests` without `limits`; GPUs are **integer, not shared or overcommitted**; one whole GPU per
- **Code delivery is different; no `rsync` into a pod.** Code is **baked into a container image**
  mount a **PersistentVolumeClaim** (or object storage) at `/checkpoints`; this is non-optional and is
  -- bash` is a debugging tool, not the run mechanism; an exec session is not durable.
  30 s**, verified kubernetes.io 2026-06) is the SIGTERM→SIGKILL window; the K8s analog of Slurm
  **node/cluster keeps costing** unless an autoscaler scales it down. **delete ≠ scale-down**; the
- **K8S1; Pod stuck `Pending`: `Insufficient nvidia.com/gpu`.** Symptom: `kubectl get pods` shows
  *usually not* missing hardware; the **device-plugin DaemonSet** isn't running, so no node advertises
  → Fix: `kubectl describe node <n> | grep -A4 -E 'Capacity|Allocatable'`; if `nvidia.com/gpu` is `0`,
- **K8S2; `RestartPolicy: Always` is rejected on a Job.** Symptom: `kubectl apply` errors that a Job's
  (restart the *container* in place; keeps `/checkpoints` warm) or `Never` (a fresh pod per attempt,
- **K8S3; `ImagePullBackOff` / `ErrImagePull` after a registry push.** Symptom: the pod never starts;
- **K8S4; `Multi-Attach error` on a rescheduled pod (RWO PVC).** Symptom: a pod stuck
- **K8S5; Pod `Evicted` mid-training under node disk pressure.** Symptom: a long run dies with
- **K8S6; Container runs but trains on CPU (GPU never attached).** Symptom: a pod runs to completion,
- **Why is it Pending / not starting?** `kubectl describe pod <p>`; the **Events** section names it
 ; `reason: OOMKilled` ⇒ raise `resources.limits.memory` (cgroup-RAM, U9); `Error` + exit code ⇒ read logs.
- **Did the node even offer GPUs?** `kubectl describe node <n> | grep -A4 Allocatable`; `nvidia.com/gpu: 0`
  section; an unbound PVC stalls the pod in `Pending`.
**K8s OVERRIDES:** `DETACH=k8s-job` · `DURABLE_DIR=/checkpoints` (PVC mount; required; RWX for multi-node)
· `CRED_FILE=""`; credentials arrive as a K8s Secret mounted as an env var (WANDB_API_KEY / HF_TOKEN),
# THIN DIFF: COLAB / KAGGLE  *(not SSH-orchestratable)*
core's central primitive ("detach + survive the session") cannot be satisfied directly; degrade to
the baseline; the work cannot be kept alive long enough.
  unpublished**; GPU type/availability and the exact ceilings "vary over time" and GPU is best-effort,
- **Free tier does not guarantee unattended background execution**; *(verified; corrects the draft's "anti-idle
  reports 2026-06). So keep-alive hacks aren't merely *unreliable*; there is **no supported headless
**Kaggle (free tier); slightly better, because of one real primitive:**
  at ~9 h GPU / ~12 h CPU). This is the closest thing to `sbatch` in the free-tier world; single it out
- **Code delivery:** clone from GitHub or pull the platform's dataset mounts; no scp.
- **NB1; Drive sync lag silently loses the "saved" checkpoint.** Symptom: training logs
  or absent** in Drive. → Root cause: writes to mounted Drive are **buffered and sync asynchronously** -
- **NB2; Kaggle commit fails if any cell errors → the whole output is lost.** Symptom: "Save & Run All"
- **NB3; Kaggle batch (commit) run picks the WRONG accelerator / has no internet.** Symptom: a committed
  internet toggle are notebook settings the commit inherits**; a notebook left on "None"/internet-off
- **NB4; `/content` (Colab) and `/kaggle/temp` are scratch, not durable.** Symptom: results written to
- **NB5; Free Colab disconnect mid-epoch with no warning.** Symptom: the session simply dies; there is
  signal; the resume contract is the *only* defense. → Fix: checkpoint every N steps to Drive
  and `!nvidia-smi`; catches a CPU-only Colab assignment or a CPU Kaggle commit (NB3) before wasting the session.
  run); a failed cell shows there; the committed `/kaggle/working` is the artifact, not the editor state.
- **Disk full inside the notebook?** `!df -h`; `/kaggle/working` caps at 20 GB; HF cache and intermediate