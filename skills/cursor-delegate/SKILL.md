You are the **orchestrator**. Hand a bounded coding task to a separate **implementer**; the Cursor
Agent CLI; then review what it produced and land it yourself. You write the brief and own the
  `--read-only` dispatch covers that; see below; but a plain review may not need delegation at all).
Omitting `--model` uses your Cursor default (usually `auto`; Cursor picks). To pin one, pass
`--model <name>` with a name from the account's live `cursor-agent models` output; select from that
Cursor sees only the text you send plus what it can inspect in the workspace; no chat history or
# read-only (plan mode: review/diagnosis, no edits):  add --read-only
tries to run is blocked; edits still land, gates do not run. Dispatch from a PowerShell or cmd
### 4. Review: do not trust the self-report
Cursor to defend or concede each; deliverable in its final message, touching no files.
- [references/writing-the-brief.md](references/writing-the-brief.md); structure, report contract,
- [references/dispatch-and-poll.md](references/dispatch-and-poll.md); flags, artifacts,
- [references/review-and-land.md](references/review-and-land.md); review checklist, commit boundary,
- [references/multi-task-queues.md](references/multi-task-queues.md); sequential queues, constraint
- Docs-only import; executable `scripts/relay.mjs` not included; see upstream for full runtime. Requires `cursor-agent` CLI, Node 18+, git.
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); docs-only, runtime not bundled.