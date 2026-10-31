Review bot feedback (from Sentry, Warden, Cursor, Bugbot, CodeQL, etc.) appears in `high`/`medium`/`low` with `review_bot: true`; it is NOT placed in the `bot` bucket.
- Never silently ignore review bot feedback; always verify the finding
- `bot` comments (informational only; Codecov, Dependabot, etc.)
- `high` and `medium` items; whether fixed or determined to be false positives
- `low` items; whether fixed or declined by the user
- Before replying, check if the thread already has a reply ending with `*- Claude Code*` or `*- Claude Code*` to avoid duplicates on re-loops
- If the `gh api` call fails, log and continue; do not block the workflow
**Wait if pending:** If review bot checks (sentry, warden, cursor, bugbot, seer, codeql) are still running, wait before proceeding-they post actionable feedback that must be evaluated. Informational bots (codecov) are not worth waiting for.
1. Read the `log_snippet` and trace backwards from the error to understand WHY it failed; not just what failed
Do NOT assume what failed based on check name alone-always read the logs. Do NOT "quick fix and hope"; understand the failure thoroughly before changing code.
If local verification fails, fix before proceeding; do not push known-broken code.
5. After all checks pass, do a final feedback check: `sleep 10`, then run `uv run ${CLAUDE_SKILL_ROOT}/scripts/fetch_pr_feedback.py`. Address any new high/medium feedback; if changes are needed, return to step 6.