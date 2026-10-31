**implementer**; the Grok Build CLI (`grok`); then review what it produced and land it yourself. You
- The task is small enough to just do inline; delegation overhead is not worth it.
   its version; the relay records the version it ran into `result.json`, so a stale binary is visible
Grok sees **only** the text you send; no orchestrator chat history, no shared context. Everything the
the project's **actual** gate commands (discover them from the repo's CLAUDE.md/AGENTS.md/Makefile -
# read-only (review/diagnosis; best-effort: verify touchedFiles): add --read-only
The helper defaults to a write-capable (`workspace-write`) autonomy profile; `--always-approve` plus
`--sandbox workspace`; and writes its artifacts to a temp dir, so the repo under review stays clean.
It **never commits**; see step 5. Mechanics, flags, and the `result.json` shape:
  the result file; `… &` in bash/zsh (including Git Bash/WSL), or your shell's equivalent (`Start-Job`
  pre-run usage error; bad args or an empty brief; instead exits with code 2 and a stderr message and
### 4. Review: do not trust the self-report
  test-guard, etc. from `guard-skills`); this skill produces the work; those skills judge it.
| *(default)* | `--always-approve --sandbox workspace` | Normal implementation; writes scoped to the working tree |
| `--read-only` | `--sandbox read-only --permission-mode plan` | Review / diagnosis; **best-effort, not enforced** (see caveat below) |
`--always-approve` alone would approve *all* tools (writes, shell, network); closer to unrestricted
filesystem/network access, not grok's own edit tool, and headless `plan` mode is advisory; a run
verified, gate-passing work is the agreed contract; that is the whole point. Two limits on that
completion needs going beyond the brief, ask; don't expand the mandate yourself). The full treatment
- [references/writing-the-brief.md](references/writing-the-brief.md); how to write a brief Grok can
- [references/dispatch-and-poll.md](references/dispatch-and-poll.md); `relay.mjs` flags, the
- [references/review-and-land.md](references/review-and-land.md); the review checklist, the commit
- [references/multi-task-queues.md](references/multi-task-queues.md); running a sequential queue:
- Docs-only import; executable `scripts/relay.mjs` not included; see upstream for full runtime. Requires `grok` CLI, Node 18+, git.
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); docs-only, runtime not bundled.