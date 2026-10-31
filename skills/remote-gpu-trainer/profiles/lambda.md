meter_stop_irreversible: true # terminate wipes local NVMe; there is no stop/suspend state
host_driver_cuda_max: lambda-stack-dependent  # Lambda Stack bundles driver+CUDA+PyTorch; version moves per release; read nvidia-smi on the box, do NOT assume a number
# Lambda Cloud: Profile
Stack** image, and **no stop/suspend state**; an instance can only be **launched, restarted, or
> **Surface to the user up front (principle #10):** ⚠️ Danger clocks; there is **no stop/suspend**: an instance can only be launched / restarted / **terminated, and terminate wipes the local NVMe**; only the attached **NFS filesystem** survives, and **it keeps billing until you delete it manually** (LAM6). Conveniences; one-click **JupyterLab** per instance, free egress both directions. A terminate→relaunch yields a **new IP**.
**Table of contents**; 1. LAUNCH · 2. STORAGE MODEL (survival matrix) · 3. NETWORK ·
Universal gotchas (CRLF, inode/`df -i`, silent sync, cgroup OOM, spot grace) are NOT repeated here -
  here if one is needed; see §2; attach any per-instance firewall ruleset here too; see §3/LAM4).
- **REST API**; `https://cloud.lambdalabs.com/api/v1`, auth via a dedicated mode-0600 netrc file selected with `curl --netrc-file "$LAMBDA_NETRC"` (basic-auth, password empty). Provision credentials privately for the confirmed API host; never put keys in command arguments, logs or shared files. Canonical automation surface (verified docs.lambda.ai/api/cloud 2026-06):
  - `GET  /instance-types`; lists every GPU type **and** `regions_with_capacity_available[]` per type.
    This field IS the capacity signal; poll it to know where a type can launch right now (drives LAM5
  - `POST /instance-operations/launch` · `.../terminate` · `.../restart`; create / stop-meter / reboot.
- **SSH**; standard connection to a normal Ubuntu VM; **default user is `ubuntu`** (not `root`); use
- **SkyPilot**; de-facto orchestration layer: `pip install "skypilot[lambda]"`, key file at
**Env contract; the image/base IS the env.** Instances ship **Lambda Stack** (NVIDIA driver + CUDA +
throwaway box; do **not** `conda create` on a rental (`references/principles.md` §2), and do not `pip
Docker inside the instance) or a setup script replayed on each launch; because terminate destroys the box.
Reserved / 1-Click Clusters provide flat-rate multi-node (own billing model; LAM12).
## 2. STORAGE MODEL  *(survival matrix: principle #4)*
- **Local / root NVMe**; fast, per-instance, **ephemeral**. Docs: *"Data not stored in the mount location
- **NFS filesystem**; a regional network filesystem mounted at `/lambda/nfs/<name>` (docs example mount:
  - **Region-locked**; *"The filesystem must reside in the same region as the instance or cluster"* and
  - **Attach-at-launch only**; *"You must attach the filesystem … at the time that the instance … is
- **No documented inode cap**; GiB quota only; no `df -i` ceiling surfaced (still audit `df -i` per the
| Local / root NVMe | `/`, `/home/ubuntu` | yes (data persists; **but cold reboot wipes RAM**; LAM9) | **NO** (erased, unrecoverable) | instance root volume |
| NFS filesystem | `/lambda/nfs/<name>` | yes | **yes** (separate lifecycle; keeps billing; LAM6) | GiB quota; ~10 TB in us-south-1, 8 EB elsewhere |
- **Direct, unproxied egress.** US/global cloud; egress to HF / GitHub / PyPI is direct; **no
- **Free egress both directions**; *"Transparent pricing with no egress fees"* (verified lambda.ai
- **Firewall**; default allows *"only incoming ICMP traffic or TCP traffic on port 22 (SSH)"*. Open more
- **Exposing TB / Jupyter**; instances get a public IP; tunnel over SSH rather than opening ports:
  `ssh -L 8888:localhost:8888 -L 6006:localhost:6006 ubuntu@<IP>`. No platform-pinned TensorBoard dir -
- **SSH flavor**; direct TCP to a normal VM (`ubuntu@<IP>`); full `scp`/`rsync` work, no proxy-jump quirk.
  **No static IP feature**; *"On-Demand Cloud doesn't support static IP addresses"* (verified DeepTalk
  (LAM10); re-read it from the console/API every launch; never hard-code it in automation.
- **Resume hook**; checkpoint full state to the NFS filesystem on a periodic timer, load-latest
**TERMINATE is the meter-stop verb; and it is irreversible.** *"Billing begins the moment you launch an
> via `terminate` from the console or `POST /instance-operations/terminate`; which works even from inside
- **terminate**; stops the instance meter; **erases the local NVMe** (unrecoverable). The NFS filesystem
  has a **separate lifecycle** and survives; but it **keeps billing $0.20/GiB/month until explicitly
- **There is no stop/suspend state**; *"It currently isn't possible to pause (suspend) your instance …
- **restart / cold reboot**; does **not** stop the meter and does **not** wipe disk, but a **cold reboot
  erases RAM and bypasses safe shutdown**; reserve it for a frozen box only (LAM9).
irreversible, an unverified `cp`/`rsync` to NFS means **permanent loss**; verify the sync (checksum /
- **Detach primitive: `tmux`** (or `screen` / `nohup`) on a standard Ubuntu VM; same playbook as the
  with no stop state the detach primitive can't survive a teardown; only the **checkpoint-to-NFS +
  instance-resilience layer. (tmux also won't survive a cold reboot; LAM9.)
  **restarts the process from scratch; the checkpoint-load restores progress** (don't assume the
## 7. TOP GOTCHAS  (Lambda-pinned: universal ones live in `references/gotchas_universal.md`)
- **LAM1; Terminate erases the local NVMe; there is no stop/suspend.**
  destroy/recreate; checkpoint to `/lambda/nfs/<name>` or `pull` off-box before any terminate; never keep
- **LAM2; Filesystem is attach-at-launch only and region-locked.**
- **LAM3; `sudo shutdown` / `poweroff` keeps the meter running (Alert state).**
- **LAM4; Per-instance firewall rulesets are immutable post-launch.**
- **LAM5; Capacity, not eviction, is the bottleneck (no spot fallback).**
- **LAM6; The NFS filesystem keeps billing after the instance is gone.**
  long as a filesystem exists, even if it's not mounted to an instance"*; $0.20/GiB/month until deleted.
  terminating attached instances first); a distinct teardown step. (billing + filesystems docs 2026-06)
- **LAM7; `pip install torch` over Lambda Stack silently shadows or mismatches it.**
  (`torch>=2.x` not `==`), or fully isolate in a fresh venv/conda env and install torch there cleanly -
- **LAM8; conda/venv that "borrows" Stack PyTorch via system-site-packages then breaks on pip.**
  the system torch) with pip installs into the same env creates two torch copies. → Fix: pick ONE model -
- **LAM9; Cold reboot wipes RAM and tmux; warm restart still bills.**
  system's safe-shutdown mechanisms"*; kills tmux sessions and any in-RAM state; neither reboot stops the
- **LAM10; No static IP; the public IP changes on terminate→relaunch.**
  *"On-Demand Cloud doesn't support static IP addresses"*; a fresh launch gets a fresh IP. → Fix: read
- **LAM11; `apt full-upgrade` on Lambda Stack images can break cuDNN/DOCA.**
- **LAM12; 1-Click Clusters / reserved bill differently than on-demand (commitment traps).**
- **LAM13; GH200 (ARM/aarch64) breaks `pip install torch`; needs the ARM build.**
  via the console or `curl --netrc-file "$LAMBDA_NETRC" https://cloud.lambdalabs.com/api/v1/instances`; an Alert-
  | {type:.key, regions:.value.regions_with_capacity_available}'`; empty `regions` ⇒ that GPU type can't
  print(torch.__version__, torch.version.cuda, torch.cuda.is_available())"`; `nvidia-smi` reports the driver-supported CUDA level, while `torch.version.cuda` reports the framework build toolkit. Different versions can be normal; check the compatibility matrix and actual CUDA errors before diagnosing shadowed packages.
  and `dpkg -l | grep -i cudnn`; confirm before debugging a "version mismatch."
  is volatile; large datasets/checkpoints filling the root volume are also *lost* on terminate, so move
- **Detect a stalled download:** background the pull (`nohup … &`) and watch growth -
  failed to come up; check the console state and prefer a fresh **terminate→relaunch** (resume from NFS)
DATA_DIR=       /home/ubuntu (ephemeral NVMe; lost on terminate)
PROXY_HOOK=     (none; direct egress; no network_turbo)
CRED_FILE=      ""  (Lambda key is the $LAMBDA_API_KEY env var, not a file on disk; run_one's [ -n "$CRED_FILE" ] guard skips the file read and the env var passes through; SkyPilot key file at ~/.lambda_cloud/lambda_keys, format `api_key = <KEY>`)
- Default checkpoint dir → the NFS mount, not `/home/ubuntu`; the latter is erased on terminate.
- Re-read the instance IP from the console/API on every launch (LAM10); never persist it in SSH config.
- Do not `pip install torch` / blanket `apt full-upgrade` on the rental; use the Stack as-is (LAM7/8/11);