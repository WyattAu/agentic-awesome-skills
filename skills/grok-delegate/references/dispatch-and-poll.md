(`<skill-dir>` is wherever this skill is installed; the folder containing its `SKILL.md`. On Claude
| `--read-only` | Review/diagnosis intent (`--sandbox read-only --permission-mode plan`). **Best-effort, not enforced**; grok can still edit the tree headlessly. The relay reports a tri-state Git-visible change tripwire. |
- `schema`; the result-format version (currently `delegate-relay.result.v1`)
- `tool`; `"grok"`
- `status`; `completed` | `failed` | `timeout` | `aborted` | `grok_unavailable`
- `exitCode`; mirrors Grok's exit code; `128` plus the signal number if the child was killed; `127` if `grok` isn't on PATH; on a `timeout` the relay forces a non-zero code even when the child exited `0` after the watchdog's SIGTERM
- `signal`; the signal that killed the child, otherwise `null`
- `grokVersion`; the binary that actually ran
- `sessionId`; feed this to a later `--session <id>` (or use `--resume-last`)
- `finalMessage`; Grok's own final report (the `<structured_output_contract>` you asked for), assembled from the streaming-json `text` events
- `usage`; token counts from the run's end event (`input_tokens` / `output_tokens` / `total_tokens`); `null` if none were reported
- `touchedFiles`; `git status --porcelain` lines in the working root: your review starting point. `null` (not `[]`) when git can't report; `[]` means git ran and the tree is clean
- `briefPath` / `eventsPath` / `finalPath`; the exact brief relay sent, the raw streaming-json event stream, and the final-message file
- `readOnlyViolation`; present on dispatched `--read-only` runs: `true` when parsed git porcelain or
  submodule internals, perfect restores, and attribution remain outside it; the diff review, not this flag, is the guarantee
- `stderrTail`; last ~20 stderr lines; present on every run that did not complete (`failed`, `timeout`, `aborted`), absent on `completed`, `grok_unavailable`, and launch failures
- `error`; present on a launch failure, and on `timeout` and `aborted` runs
- **Plain shell / other agents:** foreground for short tasks, or background and poll; `node relay.mjs
  error (bad args, empty brief) exits with code 2 *before* writing any file; so check the exit code
process has exited and `result.json` is written; not when a status line says so.
  half-applied change; inspect it before deciding between a longer `--timeout`, a smaller brief,
- **`status: failed` with `signal: "SIGKILL"`:** the host ended the child; commonly the OOM killer
  stopped; and is the source of truth for tightening the parser.
work is lost; the run killed late, or the working tree damaged afterward; read the event log
so treat any reconstruction as unverified until it matches a working-tree diff; when the tree
**Prompt delivery:** the brief is handed to grok via `--prompt-file`, never argv; so it stays out of
- **Raw `grok --prompt-file`**; fine for one-offs; you give up the captured `result.json`,
- **`grok agent stdio` (ACP)**; richer IDE/tool integration over JSON-RPC. Out of scope for this
The helper never commits; by design, not omission. The robust contract is: Grok edits the working