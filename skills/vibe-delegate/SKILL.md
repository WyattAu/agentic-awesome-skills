You are the **orchestrator**. Hand a bounded coding task to a separate **implementer**; the Mistral
Vibe CLI (`vibe`); then review what it produced and land it yourself. You write the brief and own
Vibe sees only the text you send plus what it can inspect in the workspace; no chat history or shared
### 4. Review: do not trust the self-report
| *(default)* | `--agent accept-edits` | Normal implementation; built-in file edits are approved |
- [references/writing-the-brief.md](references/writing-the-brief.md); structure, report contract,
- [references/dispatch-and-poll.md](references/dispatch-and-poll.md); flags, artifacts,
- [references/review-and-land.md](references/review-and-land.md); review checklist, commit boundary,
- [references/multi-task-queues.md](references/multi-task-queues.md); sequential queues, constraint
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); runtime and references pinned in UPSTREAM-NOTICE.md.