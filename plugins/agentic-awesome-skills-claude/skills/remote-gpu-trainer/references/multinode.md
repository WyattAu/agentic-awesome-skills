# Multi-node NCCL & elastic-training gotchas: ADVANCED
(`references/principles.md` #8; cadence + atomic-write in `references/spot-resilience.md`); multi-node only changes
grace) are **not** restated here; see `references/gotchas_universal.md`.
- **Fabric-manager**; one bad node hangs the WHOLE job at NCCL init (MN1)
- **NIC selection**; NCCL picks docker0/loopback/slow NIC (MN2)
- **Timeout masking**; default 1800 s hides a straggler/dead rank (MN3)
- **MTU mismatch**; jumbo frames silently dropped, small messages fine (MN4)
- **Elastic restart ≠ state restore**; torchrun `--max-restarts` (MN5)
- **Elastic Horovod pause-below-min-np**; pauses then errors (MN6)
- **First-move checklist**; bring up a healthy multi-node group
## MN1: Fabric-manager down on one node hangs the entire job at NCCL init
**Symptom.** Launch a multi-node job; every rank prints up to NCCL init then freezes; no traceback, no progress,
fabric-manager cannot establish its NVLink fabric, so its ranks never join the collective; and because NCCL init is
  if permitted, otherwise **stop that instance and re-rent a different box**; a fabric-manager that won't start is
## MN2: NCCL picks the wrong NIC (docker0 / loopback / a slow interface)
(`docker0`), the loopback, or the slow management NIC; none of which carry traffic between the real nodes. The
- `export NCCL_SOCKET_IFNAME=<real-iface>`; a prefix filter; e.g. `eth`, `ens`, `bond`. Exclude bad ones with a
- Confirm the chosen interface with `NCCL_DEBUG=INFO`; the `NET/Socket` or `NET/IB` line names the bound device;
## MN3: Default 1800 s NCCL timeout masks a straggler or a dead rank
`Watchdog ... collective ... timed out` / `NCCL timeout` error; or worse, hangs far longer because the watchdog is
collective waiting out the full 30-minute window before anything surfaces; so the symptom appears half an hour after
  so a crashed/unreachable rank tears the group down promptly instead of waiting out the timeout; the surviving ranks
  lower it. The default rarely fits a real job; set it on purpose.
## MN4: Jumbo-frame MTU mismatch silently drops large NCCL frames
transport error the moment a **large** payload is sent; large gradient buckets, the first big allreduce, or a model
- If the host bridge MTU cannot be changed on the rental, set the container interface **down** to 1500 to match; a
## MN5: torchrun / TorchElastic `--max-restarts` restarts the process group but does NOT restore training state
and relaunches **all** workers; but training resumes from **step 0** (or the wrong epoch), silently throwing away the
training state; that is entirely the script's responsibility. A `--max-restarts` with no in-script
- **REQUIRED:** treat the restored run as a *resume of the identical config*, never a hand-patched relaunch; a
## MN6: Elastic Horovod pauses below `--min-np`, then errors at `HOROVOD_ELASTIC_TIMEOUT`
the live count below `--min-np`; the job does **not** fail immediately; it appears to hang (paused, no progress) -
- Set `--min-np` **low enough** that the typical number of concurrent preemptions does not breach it; survivors keep
## First-move checklist: bring up a healthy multi-node group
- [ ] Distributed jobs checkpoint **more** often than single-GPU; one preemption wastes N× compute; cadence per