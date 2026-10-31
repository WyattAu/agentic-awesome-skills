# Grok Build CLI: headless reference
`grok --help` after major version bumps; flags mirror Claude Code's.
⚠️ `grok agent` is NOT a one-shot command; it runs the agent as a stdio/WebSocket
was auto-cancelled headlessly; you forgot `--always-approve` (see below).
## Permissions: the headless gotcha
Verified 2026-07-09: `--permission-mode acceptEdits` FAILS headlessly; the edit tool
network access; layer it on for untrusted repos.
Verified: the resumed session retains full context; it knows the repo and files touched
## Self-verification (`--check`): opt-in only
~48s) and adds token cost, undercutting Grok's speed/cost advantage. Skip it by default -
| `--permission-mode <m>` | `default`, `acceptEdits`, `auto`, `dontAsk`, `bypassPermissions`, `plan`; but see gotcha above |
| `--max-turns <N>` | Turn cap; always set for headless runs |