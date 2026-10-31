**implementer**; the OpenCode CLI; then review what it produced and land it yourself. You write
command and read a file, so any agent with those two capabilities; Claude Code, OpenCode driving a
sibling session, or a comparable one; can drive it. (It is designed for and run on Claude Code; treat
- The task is small enough to just do inline; delegation overhead is not worth it.
3. A model provider is authenticated; `opencode auth list` shows at least one credential.
OpenCode has **no safe default**; a bare `opencode run` errors; so a fresh run needs a model via
  the CLI can't tell them apart. So the usable set is theirs; ideally stated once in the repo's
- **You, the orchestrator, pick per task; from that set.** Match the model to the brief: a cheap, fast
- **If no usable set is stated, ask; don't guess.** Guessing from the catalog risks a metered model and
OpenCode sees **only** the text you send plus what it can read from the working tree; no chat history,
AGENTS.md/CLAUDE.md/Makefile; do not assume), and a report contract. Tell OpenCode it will **not**
writes a structured `result.json`; so your only job is "run a command, read a file." (`<skill-dir>`
below is this skill's installed directory; the folder containing this `SKILL.md`. Claude Code prints
directory; if unsure where it landed, run `find ~ -name relay.mjs -path '*opencode-delegate*'` and
repo under review stays clean. It **never commits**; see step 5. Mechanics, flags, and the
  the result file; `… &` in bash/zsh (including Git Bash/WSL), or your shell's equivalent (`Start-Job`
  pre-run usage error; bad args or an empty brief; instead exits with code 2 and writes no result
### 4. Review: do not trust the self-report
  test-guard, etc. from `guard-skills`); this skill produces the work; those skills judge it.
- **`build`** (the relay default); write-capable; edits files in the working dir headlessly. The
- **`plan`** (via `--read-only`); requests the CLI plan agent. Actual tool permissions depend on installed configuration; this flag is not a host sandbox. The
prompt no one can answer. That is the point of unattended delegation; the orchestrator's diff review
**Read-only (`plan`) runs never get `--auto`**; auto-approving would let the plan agent's ask-gated
verified, gate-passing work is the agreed contract; that is the whole point. Two limits on that
completion needs going beyond the brief, ask; don't expand the mandate yourself). The full treatment
- [references/writing-the-brief.md](references/writing-the-brief.md); how to write a brief OpenCode can
- [references/dispatch-and-poll.md](references/dispatch-and-poll.md); `relay.mjs` flags, the
- [references/review-and-land.md](references/review-and-land.md); the review checklist, the commit
- [references/multi-task-queues.md](references/multi-task-queues.md); running a sequential queue:
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); runtime helper and references bundled.