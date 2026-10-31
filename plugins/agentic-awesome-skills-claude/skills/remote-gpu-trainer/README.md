> this is an **Agent Skill**; a `SKILL.md` with reference docs and script templates; not a CLI or an
So it teaches tenant survival; detach the job, make the result outlive the box, stop the meter without
losing data; and treats that as a single model across every backend. Only the per-platform specifics
    HUB["<b>SKILL.md</b>; the always-loaded hub<br/>10 operating principles · 6-phase lifecycle · platform selector"]
    CORE --> CORE2["training/ ×8; the DL-debug layer<br/>OOM · NCCL-hang · NaN · throughput · ckpt · convergence · data"]
General orchestrators; **SkyPilot**, **dstack**, **Modal**; own or abstract the infrastructure and
**platform-agnostic core, platform-specific edges**; the principles and lifecycle hold everywhere, and
│   └── training/                #   the DL-training debug layer; when the run breaks, not the box
├── profiles/                    # one file per platform; the only place concrete specifics live
agent. It auto-triggers on remote or rented-GPU deploy / train / monitor tasks; you don't invoke it by
> them. And a few durable-monitoring recipes assume a host background-task runner plus a scheduler; map
- **`SKILL.md`**; the hub. Ten platform-agnostic operating principles, the six-phase lifecycle with a
- **`references/`**; the platform-agnostic knowledge: `principles.md` (the ten invariants expanded),
- **`references/training/`**; the **DL-training debug layer**, eight files for when the *run* breaks
- **`profiles/`**; one file per platform, the only place concrete specifics live. `autodl` is the
- **`scripts/`**; parameterized wrapper templates, a memory monitor, a GPU-health probe, a VRAM-zombie
- **`examples/autodl_sweep/`**; one complete worked case, end to end.
- **`evals/`**; a retrieval drift-guard: `cases.jsonl` holds realistic scenarios, `run_evals.py` checks
  multi-instance; long-running jobs; training, eval, ablation sweeps, batch inference, large data
The **AutoDL** profile reflects the author's hands-on, daily use. The other six profiles; RunPod,
vast.ai, Lambda, Paperspace, the Chinese platforms, and the generic SSH / Slurm / K8s core; are
and "I ran this, here's what changed" reports, are very welcome; please open an issue or PR.
MIT; see [LICENSE](LICENSE). Copyright (c) 2026 Yuyuan Han.