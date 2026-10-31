description: "Text the user's phone when a long-running task, agent turn, or scheduled job finishes; via @sendblue/cli for outbound, optionally wired to a Claude Code Stop hook for automatic fire."
- Do **not** use for short, interactive tasks where the user is watching the terminal; the notify is noise.
The user's phone number must be a verified contact on the account. On the free plan, the contact has to text the Sendblue number once before outbound sends work; confirm with `sendblue contacts` before relying on notify in an unattended workflow.
Notify is for **long, unattended work**; not chatter. Good triggers:
- Every `Stop` event, regardless of duration; produces spam, trains the user to ignore.
- **One-shot inline send**; default for a single ad-hoc task. No config changes.
- **`Stop` hook**; opt-in, project-scoped, for sessions the user explicitly wants on automatic notify. Always gate by duration.
- **End-of-`/loop` or `/schedule` ping**; append the send to the routine's body.
- **One line, under ~140 chars**; fits in the lock-screen preview.
- **Lead with outcome**; ✅/❌, "done", "failed", "needs review".
- **Include something actionable**; branch name, error tail, PR number, duration.
- **No agent self-narration** ("I have completed the task as requested"; just say what happened).
For a single task, append the send to the command. This is the default; no config changes, no surprise behavior later.
Branch on the task's exit status with an `if`/`else`. Do **not** use a `task && send-success || send-failure` chain: if the task succeeds but the success-send itself returns non-zero, the `||` fires the failure message; so the user sees ❌ even though the task completed. The `if`/`else` keeps the outcome tied solely to the task.
Register a `Stop` hook in `.claude/settings.json` (project-scoped); never in global settings unless asked. Defer the actual file edit to [[update-config]]. The hook command itself should:
2. Gate on duration; skip sends for turns under a threshold (e.g. 90s).
3. Never fail the parent; pipe to `|| true` so a notify error doesn't surface as a hook failure.
(Adjust the env var names to whatever the hook contract actually provides; verify against the current Claude Code hooks reference before writing the config; the harness owns those names, not this skill.)
If the user has `@textme` installed (njerschow/textme; daemon that lets you *text Claude* from your phone), notify is still useful and not redundant. They run in opposite directions:
- **Per-user numbers.** The destination phone number is a personal identifier; keep it in user-local config (env var, gitignored file), not in committed repo files or CI logs.
- **PII in notification copy.** Lock-screen previews are visible to anyone holding the phone. Don't embed secrets, customer data, or full error stacks; link to a log or PR instead.
- `@sendblue-cli`; Owns the actual send mechanism. This skill calls into it.
- `@sendblue-api`; HTTP alternative for app code where notify lives inside a long-running service.
- `@update-config`; Wires the `Stop` hook into `.claude/settings.json`. This skill owns the *what* and *when*; update-config owns the *where*.
- `@textme`; Inbound counterpart (phone → Claude). Composes well with notify.