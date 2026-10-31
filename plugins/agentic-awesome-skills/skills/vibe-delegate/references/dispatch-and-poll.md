commands; including most project gates; are denied headlessly rather than hanging. The orchestrator
- `brief.txt`; the exact brief.
- `events.jsonl`; raw Vibe stdout in streaming JSON format.
- `final.txt`; the last non-empty assistant message; absent if none was emitted.
- `stderr.txt`; complete stderr.
- `result.json`; the stable `delegate-relay.result.v1` contract.
- `finalMessage`; the last non-empty assistant content string; tool calls and tool results are excluded.
- `touchedFiles`; `git status --porcelain` lines for the **final working tree under `--cd`**. Not
- `stderrTail`; the last 20 non-empty stderr lines on a run that did not complete.
- `error`; present for launch failures, `timeout`, and `aborted`.
`events.jsonl` records every message Vibe streamed. If finished work is lost; the run ended late or
the tree was damaged afterward; read it before re-dispatching to scope which files and tools were