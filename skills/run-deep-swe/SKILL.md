## Prerequisites: state-check first
**Docker must be running**; Pier sandboxes each task in Docker by default (`--env modal` for cloud instead).
**Route A; native OpenRouter class (preferred, hits openrouter.ai/api/v1 directly):**
**Route B; LiteLLM provider prefix (fallback; same key):**
- Flag spelling can vary by version; confirm with `pier run --help` and `mini --help`.
## Smoke test FIRST (1 task: do this before any full run)
## Full 113-task corpus (costs tokens + time: confirm with user first)