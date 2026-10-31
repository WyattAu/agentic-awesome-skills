run never stalls on the workspace-trust prompt; point `--cd` only at repositories you trust.
- `brief.txt`; the exact brief.
- `events.jsonl`; raw cursor-agent stdout events.
- `final.txt`; the final report; absent if none was emitted.
- `stderr.txt`; complete stderr.
- `result.json`; the stable `delegate-relay.result.v1` contract.
- `finalMessage`; the `result` field of Cursor's closing event; when the run died before emitting
- `touchedFiles`; `git status --porcelain` lines for the **final working tree under `--cd` only**,
  edits Cursor makes inside `--add-dir` roots do not show up at all; inspect those trees yourself.
- `usage`; Cursor's token-usage object from the closing result event, or `null` if no result event
- `stderrTail`; the last 20 non-empty stderr lines on any run that did not complete (`failed`,
- `error`; present for launch failures, when the relay watchdog fires (`timeout`), on an `aborted`
  message is in `finalMessage`. An unknown `--model` name fails fast; re-check against
  is uncatchable (Node supports no `SIGTERM` handler there), so this status may never get written -
  not a hook bug. When cursor-agent is launched from a Git Bash (MSYS) console on Windows; which
  is what an orchestrator's bash tool uses; it selects `bash.exe` as its persistent shell while
  the command, fail-closed. File edits still work; command execution does not; which also means
work is lost; the run killed late, or the working tree damaged afterward; read the event log
so treat any reconstruction as unverified until it matches a working-tree diff; when the tree