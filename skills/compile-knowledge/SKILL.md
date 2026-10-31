description: "Compile durable, non-obvious findings into an interlinked markdown knowledge store; atomic files, [[wiki-links]], a maintained index; so an agent gets smarter across sessions instead of relearning the same facts."
time and surfaced through an index; not as one giant doc, a chat log, or a one-off
The hard part is not the format; it is the discipline of writing only what is durable, and
  investigation result, and are about to close the task; compile before you close.
- **Agent memory**; the per-agent folder your harness already loads (`memory/` with an
- **Shared wiki**; a `wiki/` folder with `wiki/index.md`, for knowledge the whole team
conversation; or if an existing file already covers it; in that last case update that file.
One fact per file. Two unrelated facts are two files. Name it as a kebab-case slug; the
Use one line in the form `- Title → slug.md; hook`, under ~200 characters. Detail lives in
- `valid_to: YYYY-MM-DD`; the date the fact needs a recheck.
- `supersedes: <slug>`; the older fact this replaces. Prefer this over editing in place
- `confidence: high|medium|low`; so a hunch never outranks a measurement.
- `provenance: "<source>"`; where the fact came from, distinct from who wrote the note.
description: "GitHub's /search/issues endpoint counts pull requests in total_count, so a zero there proves neither issues nor PRs exist; but a non-zero one does not tell you which."
- Search API counts PRs as issues → reference-search-api-counts-prs-as-issues.md; zero is query-scoped, a non-zero needs type filtering
- ✅ Delete entries that turn out to be wrong; a wrong memory is worse than a missing one.
  These files are long-lived, frequently synced, and often shared across a team; treat
  **Solution:** Bind it to a boundary you already hit; compile before closing a task, not