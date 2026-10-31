**implementer**; the Command Code CLI (`cmd`); then review what it produced and land it yourself.
- The task is small enough to just do inline; delegation overhead is not worth it.
   dispatch; a full-trust run is much easier to review against a clean baseline.
Command Code sees **only** the text you send; no repo memory, no chat history, no shared context
**actual** gate commands (discover them from the repo's AGENTS.md/CLAUDE.md/Makefile; do not assume),
writes a structured `result.json`; so your only job is "run a command, read a file." (`<skill-dir>`
below is this skill's installed directory; the folder containing this `SKILL.md`, i.e. the directory
loads; on other orchestrators use that same directory; use the explicit installation path supplied by the user or environment rather than searching the entire home directory.)
Its temp directory keeps only relay artifacts out of that repository. The relay **never commits** -
  the result file; `… &` in bash/zsh, or your shell's equivalent. The run is done when `result.json`
  exists with a `status`. (A pre-run usage error; bad args or an empty brief; instead exits with code
### 4. Review: do not trust the self-report
  nothing less? `touchedFiles` in the result is your starting point; and because the run was
  test-guard, etc. from `guard-skills`); this skill produces the work; those skills judge it.
Code to defend or concede each; deliverable in its final message, touching no files. The read-only
verified, gate-passing work is the agreed contract; that is the whole point. Two limits on that
correct completion needs going beyond the brief, ask; don't expand the mandate yourself). The full
- [references/writing-the-brief.md](references/writing-the-brief.md); how to write a brief Command
- [references/dispatch-and-poll.md](references/dispatch-and-poll.md); `relay.mjs` flags, the
- [references/review-and-land.md](references/review-and-land.md); the review checklist, the commit
- [references/multi-task-queues.md](references/multi-task-queues.md); running a sequential queue:
- Docs-only import; executable `scripts/relay.mjs` not included; inspect an approved upstream revision for runtime. Requires Command Code CLI, Node 22+, git.
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); docs-only, runtime not bundled.