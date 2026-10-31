You are the **orchestrator**. Delegate a bounded coding task to a separate **implementer**; the
GitHub Copilot CLI; then review what it produced and land it yourself. You write the brief and own
  (MXC-based, controlled via the `/sandbox` command and settings, disabled by default); this relay
   itself runs on Node 18+; the relay probes `copilot version`).
Completion means the process exited and `result.json` exists; trust process state and the
### 4. Review: do not trust the self-report
If the work is good, commit it. The relay never commits; the diff and `result.json` are the
message and a hint to pass `--allow-all-tools`. This is the honest default; the orchestrator sees
- [references/writing-the-brief.md](references/writing-the-brief.md); structure, scope, gates,
- [references/dispatch-and-poll.md](references/dispatch-and-poll.md); flags, artifacts,
- [references/review-and-land.md](references/review-and-land.md); what to verify before calling
- [references/multi-task-queues.md](references/multi-task-queues.md); sequential queues,
- Docs-only import; executable `scripts/relay.mjs` not included; see upstream for full runtime. Requires `copilot` CLI, Node 18+, git.
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); docs-only, runtime not bundled.