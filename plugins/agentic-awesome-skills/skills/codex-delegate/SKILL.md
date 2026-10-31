**implementer**; the OpenAI Codex CLI; then review what it produced and land it yourself. You write
- The task is small enough to just do inline; delegation overhead is not worth it.
   a stale Homebrew one). `command -v codex` shows the active one and `codex --version` its version -
the project's **actual** gate commands (discover them from the repo's CLAUDE.md/AGENTS.md/Makefile -
dir, so the repo under review stays clean. It **never commits**; see step 5. Mechanics, flags, and the
  the result file; `… &` in bash/zsh (including Git Bash/WSL), or your shell's equivalent (`Start-Job`
  pre-run usage error; bad args or an empty brief; instead exits with code 2 and a stderr message and
### 4. Review: do not trust the self-report
  test-guard, etc. from `guard-skills`); this skill produces the work; those skills judge it.
positions, and ask Codex to defend or concede each; deliverable in its final message, touching no
best-effort and only flagged after the fact (`readOnlyViolation`); for those implementers,
verified, gate-passing work is the agreed contract; that is the whole point. Two limits on that
completion needs going beyond the brief, ask; don't expand the mandate yourself). The full treatment
The official openai-codex Claude Code plugin is excellent and **complementary**; `codex-delegate`
  implement across one task or a queue, and *you* review and land each result. That loop; brief →
  dispatch → poll → review → commit, with the orchestrator owning the commit; is what the plugin
- [references/writing-the-brief.md](references/writing-the-brief.md); how to write a brief Codex can
- [references/dispatch-and-poll.md](references/dispatch-and-poll.md); `relay.mjs` flags, the
- [references/review-and-land.md](references/review-and-land.md); the review checklist, the commit
- [references/multi-task-queues.md](references/multi-task-queues.md); running a sequential queue:
- Docs-only import; executable `scripts/relay.mjs` not included; see upstream for full runtime. Requires `codex` CLI, Node 18+, git.
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); docs-only, runtime not bundled.