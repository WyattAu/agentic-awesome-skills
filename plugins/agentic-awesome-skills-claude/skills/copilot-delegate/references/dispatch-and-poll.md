- `brief.txt`; the exact brief.
- `events.jsonl`; raw copilot stdout events (every JSONL event copilot emitted).
- `final.txt`; the last non-ephemeral `assistant.message` content; absent if none was emitted.
- `stderr.txt`; complete stderr.
- `result.json`; the stable `delegate-relay.result.v1` contract.
- `sessionId`; parsed from the `result` event's `sessionId` field; available for resume.
- `finalMessage`; text of the last non-ephemeral `assistant.message` event.
- `touchedFiles`; `git status --porcelain` lines for the **final working tree under `--cd` only**,
- `stderrTail`; the last 20 non-empty stderr lines on any run that did not complete (`failed`,
- `error`; present on denial failures (with the CLI's own denial message plus a hint to pass
work is lost; the run killed late, or the working tree damaged afterward; read the event log
On resume (`--continue` / `--resume=<id>`) the relay wraps the reference in a fixed directive -
the file: @<file>`; because a bare `@<file>` reference in a resumed session is echoed back instead