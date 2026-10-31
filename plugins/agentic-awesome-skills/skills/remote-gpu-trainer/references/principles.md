# Operating Principles: the 10 invariants, expanded
- AutoDL, RunPod, vast.ai, Lambda, Paperspace, a Chinese platform, a bare SSH box, Slurm, or K8s. Only
passes** (principle #9 governs the *who-decides*). Every idle paid minute; a stuck download, a forgotten
box overnight, a human-in-the-loop pause on a live instance; is money.
sufficient*; it won't catch convergence; but it catches the dumb-and-expensive failures that otherwise
*Boundary:* this skill owns *when* to run the smoke (the pre-rent gate). The smoke's *content*; what to
assert, how to shrink the problem; belongs to **`verifying-dl-experiments`**. Don't duplicate it here.
"synced / saved / done / 100% complete" is a **claim**, and claims lie under a silently-failed write -
*Universal; the load-bearing spine.* It is the platform instance of
**The single biggest portability trap.** AutoDL persists `/root` across a power-off; so the AutoDL
So the principle is not a path; it's a **discipline**: for each platform, before Phase 0, read the
## 5. Storage fails on the dimension: and the location: you're not watching
is at 100%; classic on a shared FS full of many-small-files eval output). The real space hog often
lives where you didn't look; a **symlinked cache** (`~/.cache/huggingface` mapped onto the data disk)
scratch (periodic checkpoints, unused model caches; one observed sweep left **179 GB** of superseded `latest.pt`/`epoch_*.pt` while the real evidence was **<200 MB** of JSON). Pre-compute the budget; monitor `df -i`, not just
The general form of the many-small-files trap is **shard into tar** (WebDataset); see
*Universal; pure bash/tmux physics.* Identical across every SSH backend.
## 7. Design for retry: failure is probabilistic, transfers are flaky, mirrors are route-specific
than hand-patching one run (which destroys comparability; see `verifying-dl-experiments`).
**Bulk transfers are the prototypical flaky step:** wrap them in `timeout`+resume retry loops; a stall
**mirror/proxy/cache speeds ONE route, not all**; it may cover the metadata/API path while the bulk-data
Lambda/Paperspace/China the interruption is auto-shutdown/auto-release/capacity instead); see principle
mode; SSH drop, Slurm walltime kill, K8s pod reschedule, spot preemption, Colab disconnect; is:
The **detach primitive is the swappable plug**; tmux on a bare box, `sbatch --requeue` on Slurm, a Job
*Universal.* Cadence is a formula, not a guess; Young/Daly `W = √(2·μ·C)` (μ = mean time between
(SkyPilot Managed Jobs, SageMaker) move the box for you but **restart your process from scratch; your
is the **irreversible** `terminate`/`destroy` that deletes the disk; so the confirmation gate matters
Most users; especially on a platform they rent only occasionally; don't know its non-obvious
released, a stopped disk still billing), not the visible failures; surfacing them early is the cheapest