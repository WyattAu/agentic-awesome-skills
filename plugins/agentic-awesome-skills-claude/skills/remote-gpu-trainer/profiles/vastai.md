spot_available: true            # interruptible (bid) auction; central to the platform
# vast.ai: platform profile
job, and **copy results off before `destroy`**; the only verb that stops the full meter.
> **Surface to the user up front (principle #10):** ⚠️ Danger clocks; a **`stop`ped instance bills its disk FOREVER** (only `destroy` stops the full meter, and `destroy` deletes everything); **bandwidth/egress bills continuously**, host-priced. Risk; rent only **verified, high-reliability** hosts with a direct port (an unverified host can vanish mid-run); cloud-sync works even while stopped (§5), the cleanest durable target.
- §1 LAUNCH; offer-driven, Docker-image-is-the-env
- §2 STORAGE MODEL; per-machine-local disk; survival matrix; cloud-sync escape hatch
- §3 NETWORK; proxy vs direct SSH; random ports; host-set bandwidth; no China proxy
- §4 SPOT / INTERRUPTION + RESUME; bid auction, ~0 s pause, GPU-bound resume, status-poll loop
- §5 TEARDOWN / BILLING; `destroy` is the meter-stop; `stop` bills disk forever; bandwidth bills always
- §6 DAEMON TOOL; tmux dies on restart; `onstart.sh` is the durable relaunch
- §7 TOP GOTCHAS; VAST1–VAST13, platform-pinned + Platform-specific debugging
- §8 SCRIPT OVERRIDES; values to parameterize `scripts/`
Universal gotchas are NOT restated here; see `references/gotchas_universal.md`. Spot cadence math and
**Env contract; the Docker image IS the env.** A bare VM is not offered by default; the create call MUST
image ships**; a mismatch with the host driver is a real failure mode (VAST5). The image's default Python
env is the low-friction place to run; do not `conda create` on a rental (the remote-base exception holds).
docs.vast.ai/.../faq/instances 2026-06); a containerized inner runtime is not an option here.
and **re-runs on every container start**; this is the platform-native boot hook and the durable relaunch
image and boot; typically 1–5 min depending on image size** (verified docs.vast.ai CLI Hello World 2026-06);
## 2. STORAGE MODEL  *(survival matrix: principle #4)*
Three tiers; the persistence + region story is the single biggest divergence from AutoDL; **there is no
| Container / instance disk (`--disk N`) | `/` + `/workspace` | local | **yes** (bills) | **NO; gone** | fixed at create, **non-resizable**, min **10 GB** (default) |
| Cloud sync (S3 / GDrive / Backblaze / Dropbox) | off-box bucket | network | yes | **yes; fully off-box** | provider's; **works even while instance is stopped** |
| Network Volume (cross-machine) |; |; |; |; | **not in current storage docs; treat as unavailable** |
**Machine-lock; and per-GPU-lock; is the trap.** A Volume "is tied to the physical machine where created"
container disk; so the durable target is **off-box**. Two real off-box paths: (a) `vastai copy` the result
S3/GDrive/Backblaze/Dropbox; notably **works even while the instance is stopped** (verified
host-dependent** (whatever the host's Docker storage driver gives); `df -i` per host, do not assume an
**"IP Port Info" pop-up** (button on the instance) or `vastai show instance`; format
instance; discover them at runtime, never hard-code. **Hard cap 64 open ports per instance.**
**Two SSH flavors; and the scp size trap:**
  `scp` but is throttled; vast's own guidance is **scp over proxy only for transfers under ~1 GB**; above
generally fast and efficient, subject to single-link upload/download constraints"; for a multi-GB result,
**Bandwidth is metered and host-priced; NOT free by fiat (corrected).** "You are charged bandwidth prices
egress option), but a given offer **can** charge per-GB in *both* directions; read the per-offer bandwidth
fix is the job's **own** `HF_ENDPOINT=https://hf-mirror.com` / `hf_transfer`, not a platform script; see
vast.ai's **interruptible** rentals are a **live continuous-bid auction**; the cheap-GPU core of the
  (verified Rental-Types 2026-06); so the resume lever is **not** "raise this instance's bid." To recover an
  offer** (cheaper/on-demand); which is why off-box checkpoints (§2) matter.
  the original host's original GPU**; the resumability cliff (VAST3).
- **Detection signal + grace window:** **little/no advance notice; treat the grace as ~0 s, an abrupt
  `Scheduling → running` **only if the same GPU is still free** (else it sticks; VAST3), then
→ **verify:** kill-and-resume drill; `vastai stop instance <id>` then `start`; the job resumes from the last
This is the most error-prone section; be precise. (verified docs.vast.ai/.../reference/billing +
- **`destroy` is the ONLY thing that stops the full meter** (compute **and** disk). It is **irreversible** -
  indefinitely** while stopped; "stopping an instance does not avoid storage costs," "you will continue to
  is in"; so even a transfer to/from a *stopped* instance (cloud sync) accrues host-set bandwidth cost (§3).
- **On-demand instances auto-stop when their host-set lifetime expires**; "when the rental end date is
  be destroyed." There is a "short grace period where your balance may go negative before deletion occurs" -
**Teardown Iron Law (vast.ai instance):** NO `destroy` until checkpoints are **copied to local AND verified there by load** (`scripts/verify_local.py`); a cloud copy is an additional durable copy, not a substitute for this gate; the copy exit status is checked (VAST2), and the user has **explicitly approved** the
- **tmux survives an SSH disconnect but NOT a container restart/reboot/spot-resume**; a reboot or
- **SSH keys apply only to instances created AFTER the key is added**; existing instances do not get a new
  its `ssh-rsa`/`ssh-ed25519` prefix or key bytes fails key authentication; the trailing comment such as `user@host` is optional; copy the whole
  training has no managed scheduler; the orchestrator + `onstart.sh` + checkpoint-resume **is** the queue.
egress-surcharge) live in `references/gotchas_universal.md`; not repeated here.
- **VAST1; surprise bill on a "stopped" instance.** Symptom: a stopped, idle instance keeps charging for
- **VAST2; results gone after teardown.** Symptom: `destroy` run, checkpoints irrecoverable. → Root cause:
- **VAST3; paused/stopped instance stuck in `Scheduling` though the machine shows "available."** Symptom:
- **VAST4; job dies mid-step with no warning.** Symptom: interruptible run vanishes abruptly. → Root cause:
  `references/spot-resilience.md`). The bid can't be raised on a live instance; re-launch elsewhere if the
- **VAST5; CUDA driver mismatch on a fresh box.** Symptom: `torch.cuda.is_available()` is False / driver
- **VAST6; a service is unreachable on its "own" port.** Symptom: TB/Jupyter/API not reachable at the
- **VAST7; host vanishes mid-run.** Symptom: instance flips to `Offline`, work lost. → Root cause: it's a
  **marketplace**; an unverified/low-reliability host can disconnect. → Fix: filter offers on
- **VAST8; bulk `scp` over the default SSH stalls / crawls.** Symptom: a multi-GB result copy over the
- **VAST9; bandwidth shows up on the bill.** Symptom: a transfer-heavy job costs more than the GPU-hours
  alone. → Root cause: bandwidth is **host-priced and metered per byte in both directions, in every state** -
- **VAST10; disk full, and you can't grow it.** Symptom: `No space left on device` mid-run; `--disk` can't
- **VAST11; secret baked into the image or onstart-cmd is recoverable.** Symptom: a key embedded at build
- **VAST12; assuming a cross-machine Network Volume exists.** Symptom: a plan relies on a Volume following
- **VAST13; instance stuck in `Loading`, never reaches `running`.** Symptom: a new instance sits in
  `vastai show logs <id> --tail 200 [--filter <grep>] [--daemon-logs]`; uploads container logs (and, with
- **Inspect the live state machine without SSH:** `vastai show instance <id>` (or the API); compare
  `python -c "import torch; print(torch.cuda.is_available(), torch.version.cuda)"`; `False`/CPU-only ⇒ VAST5
  `vastai ssh-url <id>` prints the connection string; never assume port 22 is reachable (VAST6).
# DATA_DIR: data + (only) checkpoint mount; NOTHING survives destroy, so durable = off-box copy-out/cloud-sync
# PROXY_HOOK: none at platform level (no /etc/network_turbo). HF mirror is the JOB's own env if needed:
# CRED_FILE: empty: vast's key is the VAST_API_KEY env var, not a file. WANDB_API_KEY/HF_TOKEN also arrive via env.
# SCRATCH: what to prune (disk is fixed-size, non-resizable → prune aggressively)
# HF_HOME: redirect cache off the small root onto the data disk
# DETACH: durable relaunch is onstart.sh, NOT tmux (tmux dies on container restart/spot-resume)
image layers or the 16 KB `--onstart-cmd` (both are stored by the platform; VAST11).