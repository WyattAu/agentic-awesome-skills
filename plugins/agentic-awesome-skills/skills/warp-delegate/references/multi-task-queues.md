working tree interleave edits into a diff nobody can review; and because `oz agent run` has no
(`.agents/skills/`, `.warp/skills/`) instead; then pass it with `--skill` and stop re-pasting it.
Maintain a visible checklist; the orchestrator's task list, or a scratch file; with one line per
- **Rework**; the diff is close. Continue the conversation with `--conversation <id>` and a delta
- **Re-scope**; the task was too big or the brief was wrong. Split it and requeue the parts.
- **Abandon**; it depends on something that is not true yet. Record why and move on.
- Two consecutive tasks fail for the same underlying reason; the shared assumption is wrong, and
review the aggregate; `git diff <first-commit>~1..HEAD`:
Fix these yourself if small. If the cleanup is substantial, it is one more brief; dispatch it with