# Monitoring Patterns: durable watching of a remote GPU job
session, so layer it. Every recipe uses portable primitives; `tmux` OR `squeue` OR `pgrep`, a log
marker OR an artifact `mtime`; never one platform's paths. Bind the concrete paths/aliases from
- §0 Monitoring physics; the four facts every recipe rests on
- §3 Durable monitoring architecture; the four layers (L1 self-completion · L2 patrol · L3 sentinels · L4 handbook)
- §4 Stale-waiter hygiene; one waiter per live run, right lifetime
- §5 Two-leg self-completion; durable result sequencing + best-effort cadence
- §7 Monitoring across agent hosts; per-host background/loop/cron primitives + the 2 portability rules (Claude Code · Codex · Cursor · Trae · generic)
## §0 Monitoring physics: the four facts every recipe rests on
> Trae / …) map them to that agent's equivalents; its background-task or async runner, its own
> foreground/turn limit, its scheduler. The four-layer architecture itself is host-agnostic; the full
  ; so never foreground-poll a multi-hour run.
Corollary; **trust the artifact, not the silence.** When a job "looks done," Read its output file and
connection. **Never hold one long ssh open for the whole wait**; loop locally, reconnecting each tick.
echo "poll gave up after $MAX ticks; check ground truth manually"; exit 1
- **Quoted regex + a filename** on every remote `grep`; the two independent guards against fact 4.
- **`ConnectTimeout` / `ServerAliveInterval` / `ServerAliveCountMax`**; a dropped link self-kills fast.
- **Short connection per tick, bounded local loop with an outer wall-clock timeout**; one ssh per check, a hard tick ceiling so the
- **Detect "done" by a log MARKER, never by `pgrep`** of the waiter's own pattern; `pgrep -f` matches
> run; it is `references/gotchas_universal.md` **U39**: the panel reads a fixed logdir/port your logger
**Is the job alive? (tmux OR squeue OR pgrep; pick the profile's primitive)**
**Progress since last check**; grep the run's OWN log, not a shared master:
> **Gotcha; crash-detect on the per-run log, never the shared master.** Symptom: a poll reports "run D
**Resource pressure** (cgroup mem, GPU); thresholds are rough, profile-tunable:
- GPU at 0% but the step log advances ≠ idle; it is CPU-data-bound; sample util over several seconds,
**Disk; the silent killer**; watch `df -i` (inodes) AND `df -h` (bytes); inodes die first on
## §3 Durable monitoring architecture: the four layers (earned by three live failures)
three. Run four; **correctness in L1, liveness in L2, latency in L3, continuity in L4**:
| **L2 patrol loop** | session scheduler (cron `/loop`) | every ~30 min fire a SELF-CONTAINED patrol: one combined ssh probe + a decision table + report meaningful changes per user preference | idle gaps (NOT session death; see L4) |
| **L3 event sentinels** | session background (`run_in_background`) | the §1 short-poll `until ssh test -f MARKER; …` for minute-level reaction between patrol ticks | nothing; acceptable; L1/L2 carry correctness |
| **L4 recovery handbook** | persistent task notes (memory only with explicit user authorization) | exact resume commands, chain definitions, marker paths, "first command on reconnect"; a BRAND-NEW session takes over from one word | everything |
### L1: on-box self-completion chain (correctness)
# tmux / nohup variant: the detach primitive is the swappable plug (sbatch dependency on Slurm)
> **Gotcha; success-gate the chain markers.** Symptom: the downstream chain fires on a phantom
> marker even when a stage died; a live disk-full `torch.save` killed stage 3, the `;`-marker still
### L2: patrol loop (liveness): the design checklist (what made it actually work)
- **ONE combined ssh probe per tick**; alive-check (tmux ls / squeue / pgrep) + `*_DONE` markers + last
 ; a dir created by a killed transfer passes existence checks forever.
- **Never blind-restart**; probe session/log/markers first so a patrol firing mid-run cannot
> read-only ssh round-trip; alive + done-count + last epoch + crash-scan + `df -h`/`df -i`, an
> escalation predicate, and a one-line report even when nothing changed; parameterized from the
### L3: event sentinels (latency)
The §1 short-poll loop for minute-level reaction between patrol ticks. Survives nothing; it is the
> `run_in_background` sentinel notifies on EXIT; fine while you keep working in an ACTIVE session, but if
### L4: recovery handbook (continuity)
- **True restart-immunity means an OS-owned process** (Task Scheduler / cron); but creating one is
> **Gotcha; after a context compaction, reconcile UI task chips against the OS process table.**
## §4 Stale-waiter hygiene: one waiter per live run, right lifetime
> **Gotcha; stale background waiters pile up.** Symptom: the Background-tasks panel shows 8+ "Running"
> `until ssh grep MARKER; do sleep 20; done` waiter but never stopped the OLD one; its marker (in a
  cross-session chip from the UI; resumed-session IDs aren't stoppable programmatically).
## §5 Two-leg self-completion: durable result sequencing + best-effort cadence
"I'll check periodically" is a lie unless a trigger is ARMED; between turns the assistant does not run.
- **Leg 1; remote self-completion (survives session/SSH death while the remote host stays healthy):** the L1 chain
- **Leg 2; live progress (best-effort):** a session-bound patrol loop (L2, e.g. `/loop 30m` or cron `3,33 * * * *`)
  polling with the LOCAL ssh key. Be honest it dies when the session closes; the remote still finishes;
brittle ssh-tail, use `huggingface-skills:huggingface-trackio` (REQUIRED for that path); poll its alerts
When a probe shows trouble, pull the full traceback from the per-run log (§2) and classify; each
  `verifying-dl-experiments` (REQUIRED); this skill owns *running* the retry, not judging the number.
Universal gotchas (silent sync, CRLF, mid-run script overwrite, inode caps) are NOT restated here -
**Rule 1; the durable layer needs no agent.** L1 (the box self-completes + `touch`es a marker) plus the
box **pushing its own notification** at the end of the `&&`-chain; a `curl` webhook / email / a
`huggingface-skills:huggingface-trackio` alert; works on EVERY host, because it runs entirely on the
**Rule 2; verify the runner's actual reachability.** A cloud runner does not automatically inherit the local SSH key, VPN or private network. A local automation may reach the box using existing authorized credentials. Inspect the active tools and runner environment; never infer reachability from the product name. Use a supported secret store and least-privilege credentials where explicitly authorized, never paste a private key into prompts, logs or source.