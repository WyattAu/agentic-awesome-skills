Make a job survive being killed at a random instant; the price of riding the 50–90 %-cheaper
To jump: `grep -in '<keyword>' references/spot-resilience.md`; keywords: `grace`, `signal`, `young`,
2. [Checkpoint cadence; the Young/Daly formula](#2-checkpoint-cadence--the-youngdaly-formula)
whether the timer is the *only* durability. **The window is NOT the safety net**; see the design-breaking
| **RunPod Spot** | OS **SIGTERM → SIGKILL** (also "interruptible without notice") | **~5 s** | Far too short to flush a large checkpoint; timer is the only real durability |
| **vast.ai Interruptible** | **no signal**; bid-based; instance is *paused* (processes killed) the instant it is outbid | **~0 s (abrupt)** | Pure timer; assume cold restart + reload every time |
**Gotcha; the design-breaking one.**
Root cause: treating the grace window as the *primary* durability mechanism; it spans 2 min down to ~0
**Gotcha; GCP Preemptible 24 h guillotine.**
## 2. Checkpoint cadence: the Young/Daly formula
node is *fresh*; anything not on a cloud bucket / network volume / shared FS is gone. On a marketplace box
Managed frameworks **auto-provision a replacement** on preemption; but they restart the **process from
- **SkyPilot Managed Jobs**; strongest cross-cloud recommendation (re-provisions in a different
  preemption/hardware failures; a user-code non-zero exit is **not** auto-recovered.
- **AWS SageMaker Managed Spot**; set `use_spot_instances=True` + `checkpoint_s3_uri`; SageMaker syncs the
  **`max_wait` must be greater than `max_run`**; `max_wait` covers wait-for-capacity *plus* run time
Universal multi-cloud auto-failover is **out of scope for this skill**; use SkyPilot/dstack for that, then
`references/multinode.md`; the same invariant holds; the framework restarts processes, the per-epoch
Read this for the algorithm; adapt into the training script. The shape is platform-agnostic; only