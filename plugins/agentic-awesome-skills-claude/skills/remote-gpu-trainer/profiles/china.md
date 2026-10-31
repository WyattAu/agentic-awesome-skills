meter_stop_irreversible: mixed  # releasing the instance is; the persistent vol survives; EXCEPT LanRui 数据盘 bills while stopped
spot_available: false        # on-demand only; NO mid-run spot reclaim (see §4)
shared_fs: per-platform      # /mnt | /hy-netdisk(+/hy-nas) | work+/cloud | /home/user/netdisk/data; region/machine-scoped, see §2
inode_cap: undocumented      # size caps documented (5/20/30/10 GB free); inode caps NOT; measure df -i live
china_mirror_needed: true    # all four sit behind the GFW; mirror/proxy story is shared, not per-platform
One-line purpose: the AutoDL-shaped Chinese rentals; near-clones that share AutoDL's SSH+tmux+prebuilt-base
> **Surface to the user up front (principle #10):** ⚠️ Danger clocks (per platform, §5); a **stopped instance is auto-released** (Gpushare ~10 days, others vary) → data gone; **LanRui's 数据盘 bills while stopped**; Gpushare's **`/hy-tmp` is wiped 24 h after stop** and `/root` resets to the image. Conveniences; built-in **JupyterLab / TensorBoard** quick-tools (all four); **declare any custom port at rent time** ("高级选项"); it can't be opened later.
> are NOT restated here; see `references/gotchas_universal.md`. The mirror/proxy/download story is NOT
> restated either; it is shared across all CN platforms and lives in `references/china-network.md`.
**Env contract; the image/base IS the env; do not `conda create` on a rental.** Same rule as AutoDL, with
- **Featurize**; base is fully provisioned and used directly; `pip`/`conda install` into base persists on the
- **Matpool**; ships a **`myconda` env that auto-activates on startup** (interpreter at
  2026-06; this corrects the earlier "auto-activate off" note, which was true only for Gpushare).
- **Gpushare**; ships miniconda but **base auto-activate is *disabled*** (`登陆终端默认取消了自动进入 base 环境`).
- **LanRui**; image-provisioned (PyTorch images are a purchasable image option); base used directly.
small system disk (§2); a system-disk env is wiped wherever `/root` is ephemeral. On Gpushare specifically,
docs recommend `conda create -p /hy-netdisk/myenv` (NOT `/hy-tmp`; that auto-clears 24 h after shutdown, GS5).
## 2. STORAGE MODEL  *(survival matrix: principle #4)*
| Gpushare | `/` incl. `/root`; **resets to image on stop/restart** | **`/hy-netdisk`** (only on *marked* machines) | **`/hy-tmp`** (local SSD; auto-cleared 24 h after stop) | 20 GB sys disk |
| Persistent netdisk / 数据盘 | YES | YES (except LanRui 数据盘 still **bills**; §5) | the only safe checkpoint target |
| Gpushare `/hy-tmp` (local SSD) | **NO; auto-cleared 24 h after shutdown** | NO | fast scratch only; copy results to `/hy-netdisk` before stop (GS5) |
- **Matpool**; `/mnt` netdisk is **region-scoped**: different regions have separate netdisks that don't
- **Featurize**; code in `work`/`/cloud` persists; per common usage reports the cloud sync drive does not
  share across different regions, while *datasets* are reusable; confirm all instances of a sweep are
  same-region before fan-out. *(med confidence; official wording not re-verified 2026-06.)*
- **Gpushare**; `/hy-netdisk` exists **only on machines marked as supporting 内网存储**; an unmarked machine
- **LanRui**; `/home/user/netdisk/data` is a **per-availability-zone shared folder auto-mounted into *every*
  workspace in that zone** (`data 文件夹下的任何数据，都可以在该可用区下的所有工作空间中使用`); convenient,
FS; measure `df -i <persistent-mount>` on a live instance in Phase 0 rather than assuming a number. Redirect
before stopping; `/hy-tmp` is local AND auto-wiped 24 h after shutdown (GS5).
`references/china-network.md`; do NOT duplicate it here.** That reference owns the mirrors table
- **Gpushare; has a real academic proxy** (the closest analog to AutoDL's `/etc/network_turbo`):
  **per-session export**, NOT auto-sourced; re-run it in every new terminal/tmux pane; (b) it **whitelists
  `*.pytorch.org`, `*.kaggle.com` and *restricts every other host***; so
  `no_proxy`/route-specific trap in principle #7; validate the speed test on the same route the real transfer
- **Matpool**; no one-command egress proxy; ships source-switch scripts under `/public/script/`
- **Featurize / LanRui**; no documented one-command academic proxy surfaced; mirrors only.
at rent time** ("高级选项" on Matpool, e.g. HTTP-6006 TensorBoard / HTTP-8888); they cannot be opened
post-launch. Ports may change on restart; re-read the console, don't hard-code a port in an alias. SSH is
`ssh user@ssh.<region>.lanrui-ai.com -p <PORT> -i ~/.ssh/id_rsa` (LanRui; public-key must be uploaded to the
   pay-as-you-go instance **10 days after stop** (`实例停止 10 天后，会自动释放`; verified
   personal data + the `/hy-nas` shared storage + custom images. A stopped box is not a parked box; pull
   enabled it can stop a job that merely went quiet (e.g. between epochs with no GPU util); keep it off for
   platform; **re-verify current console paths/domain before scripting against any cached LanRui path**.
**Resume hook:** checkpoint-to-durable + load-latest-on-startup (principle #8) is still the right spine; here
**The meter-stop verb is per-platform; bind it from the table below before clicking anything.** The Iron Law
*destroyed*, even while the workspace is stopped; `工作空间停止运行，未销毁的数据盘也将持续计费` (verified
CPU-only restart, no GPU) is the analog of AutoDL's no-GPU restart; keeps `/hy-netdisk` data while paused at a
**tmux** is the family detach primitive; preinstalled on most images. Caveat (from Matpool docs, true
family-wide): **run tmux from a local SSH session, NOT the Jupyter web terminal**; keybindings collide with
on Featurize (process not killed; only notebook cell state lost); but tmux is preferred for a named,
resets `/root`, taking the tmux server and any `/root` logs with it); so the durable spine is
**multi-machine multi-GPU distributed training**; if used, see `references/multinode.md`.
**CN1; `/root` ephemerality silently loses work.**
(Featurize). → Root cause: the system disk resets to image state / is wiped on return; unlike AutoDL, which
**CN2; GPU-idle auto-stop kills a quiet job.**
**MP1; `.snap` snapshots silently consume the 5 GB netdisk.**
**MP2; `/mnt` is excluded from snapshots, and the machine is locked while saving.**
**MP3; region-scoped netdisk strands data on a sweep across regions.**
**GS1; `/root` resets to image state on every shutdown/restart.** (The instance of CN1 to remember by name.)
**GS2; turbo proxy left on blocks non-whitelisted hosts.**
**GS3; `/hy-netdisk` absent on unmarked machines.**
**GS4; stopped instance auto-released at 10 days; arrears purge at day 15.** Symptom: a parked stopped
**GS5; `/hy-tmp` auto-cleared 24 h after shutdown (and on migration).** *(NEW; corrects the prior "/hy-tmp
**FZ1; anything outside `work`/`/cloud` is wiped the instant the instance is returned.** (The strictest
**FZ2; `/cloud` sync drive lag makes edits *look* saved but not land.** Symptom: VS Code edits / files appear
**FZ3; 30 GB free cloud quota silently breaks large writes / `conda create`.** *(corrects the prior "~20 GB"
**LR1; 数据盘 keeps billing while the workspace is merely *stopped*.** (The most expensive divergence; see
(`工作空间停止运行，未销毁的数据盘也将持续计费`; verified docs.lanrui.co storage 2026-06). → Fix: to stop
all billing, stop the workspace **and** 销毁 the 数据盘; only after the Iron-Law pull+verify; only data explicitly copied to and verified on 网盘 survives destruction of the 数据盘.
**LR2; shared `netdisk/data` folder mounted into every same-zone workspace → cross-run clobber.** Symptom:
poor at many-small-file *writes*; route those to the 数据盘.
**LR3; platform/domain churn invalidates cached paths.** Symptom: scripted paths/domain fail post-migration.
  `/hy-netdisk` is present (GS3); `mount | grep hy-netdisk` (absent on unmarked machines).
  Featurize; match versions.
- **Detect a stuck / throttled download.** `du -sh <cache-dir>` twice ~30 s apart; flat size = stalled (often
- **Disk / inode pressure (the silent §2 risk).** `df -h <persistent-mount>` AND `df -i <persistent-mount>` -
- **Verify the meter-stop did what was intended.** After "stop", re-check the console billing line; on LanRui
  reset (GS1); the log must live on the persistent mount to survive.
| `CRED_FILE=""` (no file; env var) | `$WANDB_API_KEY` / `$HF_TOKEN` on **ephemeral** disk, never the shared netdisk | same | same | same |
disk, not a file on the netdisk; leave it empty so run_one's `[ -n "$CRED_FILE" ]` guard skips the file read
especially leaky; universal secrets-off-shared-FS gotcha in `references/gotchas_universal.md`).
## 9. Per-platform comparison: the load-bearing differences at a glance
| Inode cap? | undocumented; measure `df -i` | undocumented; measure `df -i` | undocumented; measure `df -i` | undocumented; measure `df -i` |
| Data disk bills while **stopped**? | no (release ends billing) | no (but stopped box auto-released at 10 d; `/hy-tmp` cleared 24 h) | no (return ends billing) | **YES; 数据盘 bills until destroyed** |
| `/root` survives a stop? | local, lost on release | **NO; resets to image** | **NO; wiped on return** | system disk lost; use 数据盘 |
`/hy-tmp` either), (2) the **meter-stop verb**; and on LanRui, that stopping is not enough, the 数据盘 must be
destroyed; and (3) the **proxy hook** (real proxy only on Gpushare, with a strict whitelist; mirrors-only