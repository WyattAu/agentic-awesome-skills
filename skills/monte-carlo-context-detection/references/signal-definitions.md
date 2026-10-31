| `dbt_project.yml` exists | Glob from workspace root | High | This is a dbt project; `prevent` skill is relevant for model edits |
| `montecarlo.yml` exists | Glob from workspace root | High | Monte Carlo monitors-as-code is configured; monitoring skills are relevant |
| User has a `.sql` model file open | IDE context / file path in conversation | High | Candidate model context; verify compiled database/schema/alias; filename alone is not warehouse identity |
1. **Prevent guardrail**; If user is editing a dbt model and verified `prevent` hooks are active, defer to it.
2. **Active skill**; If a skill or workflow is already active, do not re-route. The active skill owns the conversation.
3. **High-confidence match**; Auto-activate the matched skill/workflow.
4. **Low-confidence match**; Suggest options, let user choose.