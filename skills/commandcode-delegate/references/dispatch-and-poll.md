command -v cmd        # `cmd` is a generic name; an alias or another tool can shadow it
(`<skill-dir>` is wherever this skill is installed; the folder containing its `SKILL.md`. On Claude
| `--cd <dir>` | Working root for Command Code (default: current directory). It is the child's working directory; Command Code has no `--cd` of its own, and under `--yolo` it is a starting point, not a boundary. |
| `--effort <level>` | Reasoning effort; `low` \| `medium` \| `high`, model-dependent. The relay accepts a bare token; Command Code and the model own the supported levels. |
| `--tools-all` | Also pass `--tools-all`, so no tool stays withheld. Ignored under `--read-only`; it does not lift the write gate. |
| `--continue-last` | Continue the most recent session. "Most recent" is global, not per-repo, so an unrelated run can steal it; prefer `--session`. |
from swapping the binary mid-run. The brief goes in on stdin, never in argv; Command Code's `-p` takes
- `schema`; the result-format version (currently `delegate-relay.result.v1`)
- `status`; `completed` | `failed` | `timeout` | `aborted` | `commandcode_unavailable`
- `exitCode`; preserves Command Code's non-zero exit code; changes a zero exit with a complete non-success result to 1; uses `128` plus the signal number if the child was killed;
- `signal`; the signal that killed the child, otherwise `null`
- `commandCodeVersion`; the binary that actually ran
- `sessionId`; feed this to a later `--session <id>` (exact session; preferred) or `--continue-last`
- `finalMessage`; Command Code's own final report (the `<structured_output_contract>` you asked for),
- `resultLine`; how much of the tail survived: `complete`, `truncated`, or `absent`. See the
- `resultSubtype` / `stopReason` / `usage` / `durationMs`; straight from that result line: `success`,
- `touchedFiles`; `git status --porcelain` lines in the working root: your review starting point.
  `null` (not `[]`) when git can't report; `git` missing, or a non-repo working root; `[]` means git
- `readOnlyViolation`; only meaningful under `--read-only`: `false` when the Git-visible detector
- `autonomy`; the state the run actually got, in Command Code's terms (`--yolo …` or `plan …`)
- `briefPath` / `eventsPath` / `finalPath`; the exact brief relay sent, the raw NDJSON event stream,
  `cleanEnv`, `keepEnv`, `startedAt`, `finishedAt`; `session` is the explicit session id, or `null`
- `stderrTail`; last ~20 stderr lines; present on every run that did not complete (`failed`,
- `error`; present on a launch failure, on `timeout` and `aborted` runs, and when Command Code
## The tail is not reliable: read `resultLine`
`cmd` ends a run with a `run_end` event that embeds the **entire conversation**; every tool call,
its arguments, and its result; and then exits with `process.exit`, which discards whatever is still
pipe buffer, no matter how fast the reader is; so this is the CLI's flush behavior, not the relay's
  `stopReason`, `usage`, and `durationMs` are `null` because the CLI never delivered them; not because
  region. **The diff is the deliverable, not the report**; review `touchedFiles` and `git diff`, and
- **Plain shell / other agents:** foreground for short tasks, or background and poll; `node relay.mjs
  code 2 *before* writing any file; so check the exit code too, don't only watch for the file. (A
process has exited and `result.json` is written; not when a status line says so.
  top up; the relay's summary names which.
  half-applied change; inspect it before deciding between a longer `--timeout`, a smaller brief,
  supports no `SIGTERM` handler there), so this status may never get written; a relay process that is
- **`status: failed` with `signal: "SIGKILL"`:** the host ended the child; commonly the OOM killer or
whole conversation; every tool call, its arguments, and its result. That makes the log both the map of
is lost; the run killed late, or the tree damaged afterward; read the event log before re-dispatching.
The helper never commits; by design, not omission. Under `--yolo` Command Code *can* write `.git`,