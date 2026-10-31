(`<skill-dir>` is wherever this skill is installed; the folder containing its `SKILL.md`. On Claude
| `--model <name>` | Model as `provider/model`. **Required on a fresh run**; OpenCode has no safe default (a bare `opencode run` errors); a resumed run inherits its session's model. |
| `--read-only` | Shortcut for `--agent plan`; review/diagnosis with no edits. |
- `schema`; the result-format version (currently `delegate-relay.result.v1`)
- `tool`; `opencode`
- `status`; `completed` | `failed` | `timeout` | `aborted` | `opencode_unavailable`
- `exitCode`; mirrors OpenCode's exit code; `128` plus the signal number if the child was killed; `127` if `opencode` isn't on PATH; on a `timeout` the relay forces a non-zero code even when the child exited `0` after the watchdog's SIGTERM
- `signal`; the signal that killed the child, otherwise `null`
- `opencodeVersion`; the binary that actually ran
- `agent`; the agent selected for this dispatch (`build`, `plan`, …)
- `sessionId`; feed this to a later `--session <id>` (or use `--resume-last`)
- `finalMessage`; OpenCode's assembled final text (the `<structured_output_contract>` you asked for).
  Empty if OpenCode stopped without emitting a closing summary; ask for the report explicitly
- `touchedFiles`; `git status --porcelain` lines in the working root: your review starting point.
  `null` (not `[]`) when git can't report; `git` missing, or a non-repo run; `[]` means git ran and
- `cost`; total run cost in USD, summed from the step events (`null` if none were reported)
- `briefPath` / `eventsPath` / `finalPath`; the exact brief relay sent, the raw JSON event stream, and
- `stderrTail`; last ~20 stderr lines; present on every run that did not complete (`failed`, `timeout`, `aborted`), absent on `completed`,
- `error`; present on a launch failure, and on `timeout` and `aborted` runs
- **Plain shell / other agents:** foreground for short tasks, or background and poll; `node relay.mjs
  error (bad args, empty brief) exits with code 2 *before* writing any file; so check the exit code
process has exited and `result.json` is written; not when a status line says so.
  half-applied change; inspect it before deciding between a longer `--timeout`, a smaller brief,
- **`status: failed` with `signal: "SIGKILL"`:** the host ended the child; commonly the OOM killer
  completes purely through tool calls). The edits may still be correct; check `touchedFiles` and the
  headless mode. Runs pass `--auto` by default precisely to avoid this; so a hang almost always means
work is lost; the run killed late, or the working tree damaged afterward; read the event log
so treat any reconstruction as unverified until it matches a working-tree diff; when the tree
The brief is fed on **stdin**, never as an argument; which is why a multi-line, XML-tagged brief needs
If you ever want it, raw `opencode run` is fine for one-offs; you just give up the captured
The helper never commits; by design, not omission. The robust contract is: OpenCode edits the working