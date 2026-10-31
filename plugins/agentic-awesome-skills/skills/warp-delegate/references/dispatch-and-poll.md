- `brief.txt`; exactly what was sent.
- `events.jsonl`; the raw ndjson stream, byte for byte. The fallback whenever a parsed field looks
- `final.txt`; the assembled report, when one was captured.
- `stderr.txt`; everything Warp wrote to stderr.
- `result.json`; the structured result, written atomically via a temp file and rename, so a polling
- **`status`**; `completed`, `failed`, `timeout`, `aborted`, or `warp_unavailable`.
- **`touchedFiles`**; `git status --porcelain` lines. `null` when git cannot report (not a
  things; do not collapse them.
- **`runId` / `runUrl`**; from Warp's `run_started` system event. `runUrl` opens the run in Warp.
- **`conversationId`**; the handle to pass back as `--conversation` for rework. Present only when
- **`finalMessage`**; Warp's own report. Assembled from the text-bearing events in the stream; see
- **`stderrTail`**; last 20 stderr lines, included on every non-clean outcome.
  final-message event, so every `agent` chunk is concatenated; including running commentary like
Past those two rules the extraction stays tolerant; it also accepts `agent_output`, `content`, and
a nested `message.content`; so a renamed output event still reports rather than yielding an empty
| `2` | Usage error; bad flag, bad `--timeout`, missing or empty brief. **No result file is written.** |
stdout going quiet; a long tool call looks identical to a finished run.
than stashing; a bare `git stash` leaves behind every file the run created. See