actually ran into `result.json`; if something behaves oddly, check which binary answered.
(`<skill-dir>` is wherever this skill is installed; the folder containing its `SKILL.md`. On Claude
| `--read-only` | Shortcut for `--sandbox read-only`; review/diagnosis with no edits. |
| `--resume-last` | Continue the most recent Codex session; send only the delta brief (see review-and-land). "Most recent" is global, so an unrelated Codex run can steal it; prefer `--session`. |
- `schema`; the result-format version (currently `delegate-relay.result.v1`)
- `status`; `completed` | `failed` | `timeout` | `aborted` | `codex_unavailable`
- `exitCode`; mirrors Codex's exit code; `128` plus the signal number if the child was killed; `127` if `codex` isn't on PATH; on a `timeout` the relay forces a non-zero code even when the child exited `0` after the watchdog's SIGTERM
- `signal`; the signal that killed the child, otherwise `null`
- `codexVersion`; the binary that actually ran
- `threadId`; feed this to a later `--session <id>` (exact thread; preferred) or `--resume-last` (global "most recent", which another Codex run can steal)
- `finalMessage`; Codex's own final report (the `<structured_output_contract>` you asked for)
- `touchedFiles`; `git status --porcelain` lines in the working root: your review starting point. `null` (not `[]`) when git can't report; `git` missing, or a non-repo run under `--skip-git-repo-check`; `[]` means git ran and the tree is clean
- `briefPath` / `eventsPath` / `finalPath`; the exact brief relay sent, the raw JSONL event stream, and the final-message file
- `workdir`, `sandbox`, `model`, `effort`, `resumeLast`, `session`, `cleanEnv`, `keepEnv`, `startedAt`, `finishedAt`; `sandbox` is the applied mode, or a note that Codex used its active config on an unqualified resume; `session` is the explicit session id, or `null` for fresh and `--resume-last` runs; `keepEnv` records names only, never values
- `stderrTail`; last ~20 stderr lines; present on every run that did not complete (`failed`, `timeout`, `aborted`), absent on `completed`, `codex_unavailable`, and launch failures
- `error`; present on a launch failure, and on `timeout` and `aborted` runs
- **Plain shell / other agents:** foreground for short tasks, or background and poll; `node relay.mjs
  error (bad args, empty brief) exits with code 2 *before* writing any file; so check the exit code
process has exited and `result.json` is written; not when a status line says so.
  half-applied change; inspect it before deciding between a longer `--timeout`, a smaller brief,
- **`status: failed` with `signal: "SIGKILL"`:** the host ended the child; commonly the OOM killer
work is lost; the run killed late, or the working tree damaged afterward; read the event log
what needs redoing. It cannot rebuild the changes themselves; Codex's JSON stream currently
reports a file change as its path and kind only, without the diff contents; so when the tree
- **Raw `codex exec`**; fine for one-offs; you give up the captured `result.json`, touched-files
- **The openai-codex Claude Code plugin's companion CLI** (`task`/`status`/`result`); richer job
  in-process and blocks until completion, so the only state to track is whether `result.json` exists -
The helper never commits; by design, not omission. Whether Codex's sandbox can write `.git` varies by