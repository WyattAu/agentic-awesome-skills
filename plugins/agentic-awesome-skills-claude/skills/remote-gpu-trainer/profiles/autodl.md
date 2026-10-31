The deepest, battle-tested profile; a Chinese cgroup-isolated SSH-rental with a 3-tier storage model
verb, and TB pin the SKILL.md phases delegate to. Universal gotchas are NOT restated here; see
> **Surface to the user up front (principle #10):** conveniences most users miss; the console has a
>; **关机 (stop) auto-releases the box after 15 days → the data disk is deleted** (AD-DANGER, §5); only
> `/root/tf-logs`**; write your logger there (or symlink) or the panel shows empty (AD7 / U39).
1. LAUNCH; entry points + env contract (base miniconda IS the env)
2. STORAGE MODEL; 3 tiers + survival matrix + inode cap
3. NETWORK; academic proxy + China mirrors + pinned TB
4. SPOT / INTERRUPTION + RESUME; effectively on-demand
5. TEARDOWN / BILLING; 关机 stops the meter AND keeps the disk (the AutoDL exception)
6. DAEMON TOOL; tmux / nohup
7. TOP GOTCHAS; AD1..AD9, platform-pinned
8. SCRIPT OVERRIDES; values to parameterize `scripts/`
meter_stop_irreversible: false  # the AutoDL EXCEPTION; 关机 is reversible; only 释放/release deletes
shared_fs: true                 # /root/autodl-fs; region-locked, cross-instance within one region
china_mirror_needed: true       # behind the GFW; hf-mirror / ModelScope + /etc/network_turbo
(choose the PyTorch image; the base env ships prebuilt) → register your key once via **设置SSH免密登录**
every phase below. (Console-only steps; AutoDL's UI shifts; re-check its docs if a label moved.)
job control; SSH is the orchestration channel. Set a stable alias per instance in `~/.ssh/config`
**Env contract; the prebuilt base miniconda IS the env (AD6).** The image ships the full DL stack into
deliberate single-tenant project env. **Never `conda create` / `conda clone base`** on the rental -
> The "no DL in conda base" discipline applies to the *persistent local* machine only; on an ephemeral
## 2. STORAGE MODEL  *(survival matrix: principle #4)*
**Survival matrix**; the part most platforms get wrong, and where AutoDL is the **exception**:
| `/` system | **yes** | no | AutoDL persists `/root` across power-off; UNLIKE RunPod/vast/K8s/Colab |
to `/root/autodl-fs`**; the only tier that survives a 释放. If only ever using 关机, the data disk also
the data disk explicitly (see §8). Disk is expandable; prefer expand over silently shrinking the
**Egress proxy; `source /etc/network_turbo` is MANDATORY (AD1).** Instances start with no proxy; direct
vs >120 s timeout without; no exceptions, even a small `wandb.summary` write can wedge for minutes.
proxy; (b) `no_proxy` in network_turbo lists `modelscope.com` but **not** `modelscope.cn`; routing a
pid; the `--logdir` is hard-pinned and cannot be reconfigured from inside the container. Events written
has `--reload=5`, so the run appears within ~5 s; no restart). Verify with
inside the container while curl returns 200). Local logs die with the instance; for durable curves use a
**SSH flavor.** Direct-TCP SSH on the per-instance host:port; `scp`/`rsync` work normally (no proxied-SSH
not always OOM; enumerate reboot / OOM / SSH-HUP / manual-kill before concluding, see
`references/gotchas_universal.md`); (c) availability; the GPU plan being sold out at create-time (build
formula → `references/spot-resilience.md` (the formula generalizes even without spot; it bounds
**关机 (shutdown / power-off) STOPS the meter AND keeps `/root` + both disks; this is the AutoDL
| 关机 (shutdown) | **yes** | **yes** | yes | **yes**; restart anytime (the AutoDL exception) |
| 释放 (release/destroy) | yes | **NO** | yes | **NO; deletes `/` + data disk irreversibly** |
**Cost trap.** 关机 still bills the data-disk *storage* at a small rate while the GPU meter is off; far
(principle #10)**; most users assume 关机 parks the box indefinitely.
verified by load** (`scripts/verify_local.py`) AND the user explicitly approves; "it looked done in the
`nohup bash run_queue.sh queue.txt </dev/null >master.log 2>&1 &`; survives an SSH drop (SIGHUP), needs
survive a 关机/reboot; that is what checkpoint+resume (§4) is for.
blocks; version the filename); universal physics, see `references/gotchas_universal.md`.
loops forever). A cloud scheduler cannot reach the rented box (no SSH key in a cloud sandbox; secret
**AD1; external network call hangs / wandb shows 0 runs.** *Symptom:* `wandb.init` times out at
**AD2; HF download stalls even with hf-mirror + turbo.** *Symptom:* `from_pretrained` /
**AD3; cross-region instances cannot share FS.** *Symptom:* two instances in different regions see
**AD4; FS write fails "No space left" while `df -h` looks fine.** *Symptom:* `cp`/`mkdir` to
**AD5; data disk full; HF cache is the hidden hog; agent `rm` auto-denied.** *Symptom:*
**AD6; base IS the env; a "never use base" rule blocks every remote command.** *Symptom:* a local "don't
the whole DL stack into **base**; base IS the single-tenant project env (no `/envs/`), and the image often
**AD7; platform TensorBoard pinned to `/root/tf-logs`; events elsewhere invisible.** *Symptom:* the
**AD8; wandb val-phase CPU memory spike to 30+ GB at epoch 1 end.** *Symptom:* at the end of epoch 1
~30 GB activations). *Fix:* cap the val-time sample accumulator; `-o training.val_metric_sample_cap=256`
**AD9; project torch pin would DOWNGRADE the image's working build.** *Symptom:* the image ships e.g. a
*Fix:* filter framework pins out of the remote install -
/root/req_remote.txt`; keep the image build; smoke `torch.cuda.get_device_capability()` + a heavy import
DATA_DIR=/root/autodl-tmp             # fast NVMe data disk; live checkpoints, logs, HF cache
DURABLE_DIR=/root/autodl-fs           # region-locked shared FS; the only tier surviving 释放
CRED_FILE=/root/.wandb_key            # per-instance ONLY; the FS security classifier blocks wandb keys
PY=/root/miniconda3/bin/python        # base IS the env; explicit interpreter, never bare python3 (AD6)
**Credential push (AD-specific).** The FS security classifier blocks files matching wandb-key patterns -
`$DURABLE_DIR/final_ckpts/<name>` **gating the success echo on the actual copy result**; an unconditional