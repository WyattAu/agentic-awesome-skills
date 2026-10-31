You are the **orchestrator**. Delegate one bounded coding task to a separate **implementer**; a Claude
Code CLI session; then review what it produced and land it yourself. You write the brief and own the
provides a cross-orchestrator contract; self-contained brief → dispatch → artifacts → review → land
- and keeps the commit with the orchestrator.
- [references/writing-the-brief.md](references/writing-the-brief.md); context, `CLAUDE.md` versus
- [references/dispatch-and-poll.md](references/dispatch-and-poll.md); flags, profiles, artifacts,
- [references/review-and-land.md](references/review-and-land.md); generated-code review, the commit
- [references/multi-task-queues.md](references/multi-task-queues.md); sequential queues, progress
- Docs-only import; executable `scripts/relay.mjs` not included; see upstream for full runtime. Requires `claude` CLI, Node 18+, git.
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); docs-only, runtime not bundled.