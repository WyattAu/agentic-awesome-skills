user in a fixed 4-part format → verify against a checklist before reporting done -
| Launch Cline inside the user's dev environment (conda env, pinned branch) | Make unilateral technical decisions; hard-constraint actions always go back to the user first |
| Monitor long background jobs via session files + `git`/test-report evidence, not self-reports | Push, delete, write to DB, spend money, change global config; always asks first |
deterministic code; the agent doesn't improvise monitoring each time.
- **Cold-start gate**; before any memory bank is activated, the default global
- **Two cold-start paths**; no code yet: rules are assembled by asking the user
- **No architecture in the skill**; project technical facts belong to the project's
- **Deterministic first**; anything that must be right every time is a script, not a
- **Evidence over self-report**; completion is proved by git status, test-report
- **Learning loop**; correction → `decision-log.md` (per tag class) → ≥2 consistent
- **Privacy by layer**; public files (SKILL.md, scripts, templates) carry zero
1. **Initialization; use a strong long-context (paid) model.**
   This phase is read-heavy and long-context-heavy; exactly where frontier
2. **Steady state; switch to a small local model for the task loop.**
   `qwen3.8:27b` locally via Ollama for all batch execution; 50+ test
row, that is a signal the template/rules are the gap; escalate *that batch*
MIT; see [LICENSE](https://github.com/gongdear/cline-pilot/blob/main/LICENSE)