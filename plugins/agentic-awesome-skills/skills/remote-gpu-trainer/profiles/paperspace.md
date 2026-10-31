# Paperspace (DigitalOcean): platform profile
notebooks/deployments) and Paperspace Core (raw Linux VMs, "Machines"); what stops the meter, what
NOT repeated here; see `references/gotchas_universal.md`.
> **Surface to the user up front (principle #10):** ⚠️ Danger clocks; an **an enabled auto-shutdown timer can end a Notebook/Core run** (set it consciously; Gradient free notebooks hard-cap at 6 h); **snapshots / block storage keep billing after a machine is destroyed** (orphan bleed). Heads-up; the **Gradient CLI/API was deprecated 15 Jul 2024** (verify a currently supported tool; the tooling split, §1).
1. LAUNCH; Gradient vs Core, the env contract, the three-CLI mess
2. STORAGE MODEL; survival matrix, the stop-keeps-disk rule, pip-doesn't-persist
3. NETWORK; public IP (static vs dynamic), ports, SSH flavor
4. SPOT / INTERRUPTION + RESUME; the auto-shutdown clock, not spot
5. TEARDOWN / BILLING; what actually stops the meter (the trap)
6. DAEMON TOOL; tmux on Core; why Notebooks resist a daemon
7. TOP GOTCHAS; `PS1`–`PS13`, platform-pinned + platform-specific debugging
8. SCRIPT OVERRIDES; values for the `scripts/` templates
- **Gradient**; the managed layer. **Notebooks** are a web Jupyter IDE on a shared persistent store;
- **Core / Machines**; raw Linux/Windows VMs with a persistent block disk, full root/SSH. OS templates
**Env contract.** The chosen image/template IS the Python env; do NOT `conda create` on a rental
`/usr/local/lib`, ephemeral); see §2 / `PS3`.
- The **legacy Gradient REST API endpoints were deprecated 15 Jul 2024**; stale calls 404 or no-op.
- **`gradient-python` (github.com/digitalocean/gradient-python) is NOT the orchestration CLI**; it is the
  new DigitalOcean *Gradient AI / GenAI inference* SDK. **Name collision**; do not install it expecting
## 2. STORAGE MODEL  *(survival matrix: principle #4)*
The defining fact: a **stop/shut-down keeps the disk**; Paperspace is one of the few profiles here that
**Gradient Notebooks**; `/storage` and `/notebooks` are **separate branches from `/`, NOT nested**
- `/storage`; **shared persistent**, team-wide, scoped to a **storage region/cluster**. Survives stop.
  (Team-shared ⇒ never write secrets here; see §7 / `references/gotchas_universal.md`.)
- `/notebooks`; **per-notebook persistent**, managed via the console File Manager. Survives stop.
- everything else; **ephemeral workspace** (incl. `/usr/local/lib` where `pip` lands), wiped on stop.
**Core machines**; block storage **50 GB–2 TB**, persists across a stop; **expansion is one-way**
(`$0.29/GB/mo`, default policy is **"Never" / 0 stored**; they bill only if manually enabled, and a
snapshot **survives a machine destroy**, so an orphaned snapshot keeps charging; see `PS9`).
cross-delete-of-the-notebook); `/notebooks` dies if the notebook itself is deleted. On Core, the block
  documented egress fee. China-mirror relevance is **N/A as a platform feature**; relevant only when
  - **Static**; "the same IP address every time it powers on … remains in your account until you delete
    it." Use it to pin stable SSH/endpoint addressing. **Billed until deleted**; *including while the
    dynamic IP to an existing machine**; dynamic must be requested at machine-creation time.
  - **Dynamic**; "assigned automatically when a machine powers on and deleted when it powers off"; a **new
- **Ports / services.** Firewall is self-managed; open ports to expose services. Tunnel Jupyter (8888) /
  (placeholder host; substitute the machine's real IP/static address). In a Gradient Notebook, launch
  `paperspace`). Gradient Notebooks expose a **Jupyter sandbox**, not a clean persistent SSH daemon -
1. **Capacity-at-launch.** The desired GPU type may be unavailable when launching; a *launch-time*
2. **Auto-shutdown clock; the hard ceiling on any long run.** The timer is the real killer:
   - **Trap (Core/Linux):** Core Linux auto-shutdown is **wall-clock, not idle-based**; "Linux machines
   - **Trap (API):** auto-shutdown **cannot be enabled/disabled via API or CLI on an existing machine** -
   Because the clock is known in advance, cadence can be planned rather than guessed; but the
## 5. TEARDOWN / BILLING  *(principle #9 + the Iron Law: the most error-prone section)*
compute (GPU) meter** while disk persists; this is the AutoDL-like part. **But it does NOT stop every
  **maximum monthly charge**; "once a non-GPU resource reaches its monthly maximum, it no longer incurs
  **delete the storage** (AND delete any **snapshot**); separate actions. "To stop all charges for a
> user has **explicitly approved** the specific cost-affecting action. A destroy is irreversible; "it
- **Core machines**; full VMs ⇒ `tmux`/`screen`/`nohup` all available; SSH is as stable as any cloud VM.
  survive a machine **stop/restart** (the process is gone), and; critically on Core/Linux; a live tmux
- **Gradient Notebooks**; a managed Jupyter sandbox: **no clean persistent SSH-daemon story**, and the
  **auto-shutdown timer is a hard ceiling**; a tmux session started inside a Notebook **still dies at the
- **Platform-native long-job mechanisms**; **Workflows** (DAG automation) and **Deployments** (always-on
- **PS1; "Stopped the machine, still getting billed."**
  snapshot**; separate teardown actions. Audit for orphaned storage/IPs/snapshots after every stop.
- **PS2; A long run dies at a round-number wall-clock with no error.**
  Root cause: the **auto-shutdown clock**, not a crash; free notebooks 6 h (hard cap), paid notebooks 12 h
  default, Core 1 h–1 wk. On Core/Linux the clock is **wall-clock, not idle**; an active SSH/tmux session
- **PS3; `pip install` (or any non-`/storage` write) vanishes after a Notebook restart.**
  Root cause: `pip` writes to `/usr/local/lib`, which is **ephemeral workspace**; only `/storage` and
- **PS4; Automation 404s / silently no-ops / installs the wrong SDK.**
  **`gradient-python` v3 is the DigitalOcean Gradient AI inference SDK; a name collision**, not the
- **PS5; Custom template / storage / volume "not found" in a different datacenter.**
- **PS6; SSH alias breaks after every restart.**
  Fix: attach a **Static IP** for stable SSH/endpoint addressing (it bills until deleted, capped $3/mo -
- **PS7; Free-tier notebook code is PUBLIC by default.**
- **PS8; Free notebook won't start / sits "pending."**
  are pooled; a pending notebook is queued for the next free machine (verified Paperspace free-instances
- **PS9; A destroyed machine keeps billing via a leftover snapshot.**
- **PS10; Notebook upload/import fails on the 5 GB free cap.**
- **PS11; ML-in-a-Box CUDA/driver too old for current PyTorch on a new-arch GPU.**
- **PS12; Gradient Deployment / custom image won't pull or drifts.**
- **PS13; Platform-specific debugging.** Commands + what to check (Core uses standard Linux tooling; the
    `python -c "import torch;print(torch.__version__, torch.version.cuda, torch.cuda.is_available())"` -
    mounts; anything else (incl. `/usr/local/lib`) is ephemeral (`PS3`).
    machines, **public IPs**, **storage/volumes**, and **snapshots**; `PS1`/`PS9` hide in the last two.
host/IP (never a real address). Core and Gradient differ; both shown.
DURABLE_DIR=/storage             # checkpoints land here (NOT /notebooks; dies with the notebook)
SCRATCH=/tmp                     # ephemeral workspace; wiped on stop; never the only copy
PROXY_HOOK=                      # none; direct egress (no network_turbo)
CRED_FILE=""                     # Paperspace keys are Gradient secrets / env vars, not files; WANDB_API_KEY/HF_TOKEN arrive via the secret/env (run_one's [ -n "$CRED_FILE" ] guard skips the file read); never write keys to /storage (team-shared)
DATA_DIR=/path/to/blockstore     # placeholder; the attached block disk mount
CRED_FILE=""                     # Paperspace keys are Gradient secrets / env vars, not files; WANDB_API_KEY/HF_TOKEN arrive via the secret/env (run_one's [ -n "$CRED_FILE" ] guard skips the file read); inject at launch, never inline
DETACH=tmux                      # survives SSH drop, NOT a machine stop, and NOT the wall-clock auto-shutdown; rely on checkpoint+resume
SSH_HOST=<machine-ip>            # placeholder; ML-in-a-Box user is `paperspace`; pin a Static IP for a stable alias (PS6); dynamic IP changes every start
Reminder: secrets referenced by env-var NAME or Gradient secret only; never inline a key, and never write