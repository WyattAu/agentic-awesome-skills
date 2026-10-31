the workspace; no chat history, no shared context, none of the reasoning that led you here.
  brief that inlines whole files can trip it. Reference paths rather than pasting file contents -
1. **Goal**; one sentence on the outcome, not the mechanics.
2. **Current state**; where the code is now, and the paths that matter. **Name the workspace root
   file-reading tool resolved bare relative paths against `$HOME`; so `src/strings.js` was first
3. **What to change**; the specific edits, in the order they make sense.
4. **What to leave untouched**; files, patterns, and public interfaces that must not move. Warp has
5. **Gates**; the project's *actual* commands. Read them out of `package.json`, `Makefile`, or the
6. **Report contract**; what the final message must state (below).
7. **Do not commit**; say it explicitly. Committing is the orchestrator's job.
claim to verify, never as verification; see [review-and-land.md](review-and-land.md).
task. Anything not in one of those locations; a plain `CONTRIBUTING.md`, for instance; must be
conversation you cannot resume precisely. Split them and queue the parts; see
- What you reviewed and what was wrong; be specific about the file and the symptom.