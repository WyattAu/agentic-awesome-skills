you're about to do. The skill bodies hold the actual guidance; invoke them with the
releases. When you spot drift; a tool a skill names doesn't exist, a parameter shape
doesn't match what `get_node` returns, behavior differs from what a skill describes -
1. **Invoke the relevant skill before any n8n action**; not just before MCP calls.
   wired. Validation passing means the JSON is well-formed; not that the workflow is
n8n changes constantly. "Remembered" parameter names are often silently wrong; they
| "This workflow is simple, I'll just build it" | `n8n-workflow-patterns`; choose the smallest correct graph, not an arbitrary node-count target |
| "I'll add a Set node to map these fields" | `n8n-expression-syntax`; Set feeding ≤1 consumer is the #1 antipattern |
| "I'll just use a Code node, it's easier" | `n8n-code-javascript`; the bar is high; most reaches are expressions or Edit Fields |
| "The user mentioned data, I'll write Python" | `n8n-code-javascript`; default JS; Python (`n8n-code-python`) only on explicit ask |
| "I'm writing code an AI agent will call" | `n8n-code-tool`; a different runtime contract from the Code node |
| "Date math; I'll drop in a DateTime node" | `n8n-expression-syntax`; Luxon inline is almost always right |
| "I'll wire a Merge with 3 sources" | `n8n-node-configuration`; Merge defaults to 2 inputs; the 3rd silently drops |
| "Validation passed, I'm ready to activate" | `n8n-validation-expert` + `n8n-workflow-patterns`; run the antipattern scan |
| "Validation threw an error I don't understand" | `n8n-validation-expert`; what each error and warning means, and which are must-fix vs. best-practice advice |
| "I'll reference `$json.x` here" | `n8n-expression-syntax`; prefer `$('Node').item.json.x` in branchy workflows |
| "This webhook/scheduled flow is happy-path only" | `n8n-error-handling`; wire an error branch on every fallible node; 4xx caller faults, 5xx yours |
| "I'll pass this file/image through as JSON" | `n8n-binary-and-data`; file contents live in `$binary`, and can't cross the agent-tool boundary |
| "I'll wire up an AI agent and give the model some tools" | `n8n-agents`; tool names & descriptions ARE the prompt; memory, structured output, and topology have traps |
| "I'll copy this logic into another workflow" / "this is getting big" | `n8n-subworkflows`; extract a reusable sub-workflow; search before building |
| "I'll create that credential / open that workflow" (account has >1 instance) | `n8n-multi-instance`; every call hits the currently-targeted instance; reads misroute silently, and an ambiguous credential write fails closed with `INSTANCE_AMBIGUOUS` |
| `n8n-code-tool` | The AI-agent-callable Custom Code Tool (`toolCode`); returns a string, no `$fromAI`/`$input` |
## n8n-mcp tools: working knowledge from turn one
- `tools_documentation`; meta-docs for every tool; `{topic:"ai_agents_guide", depth:"full"}` for the agent guide.
- `search_nodes`; find nodes by keyword.
- `get_node`; node info. Takes a single **SHORT-form** `nodeType` (`nodes-base.httpRequest`, `nodes-langchain.agent`), plus `detail` (minimal/standard/full) and `mode` (info/docs/search_properties/versions).
- `validate_node`; validate one node's config in isolation (profiles: minimal/runtime/ai-friendly/strict).
- `search_templates` / `get_template`; the template library (by keyword, nodes, task, metadata).
- `n8n_create_workflow`; create from full workflow JSON.
- `n8n_update_partial_workflow`; incremental diff ops (`{id, operations:[…]}`): addNode, updateNode, patchNodeField, addConnection, activateWorkflow, etc. Preferred for edits.
- `n8n_update_full_workflow`; full replacement.
- `n8n_autofix_workflow`; auto-fix common issues.
- `n8n_deploy_template`; deploy a template to the instance.
**Validate** (necessary, not sufficient; always pair with the antipattern scan)
- `validate_workflow`; full JSON in, errors/warnings/fixes out. Node types here are **LONG form** (`n8n-nodes-base.set`).
- `n8n_validate_workflow`; validate a deployed workflow by `{id}` (no node JSON to inspect).
- `n8n_get_workflow`; fetch a workflow (full / structure / active / filtered / minimal). Use it to verify `connections` after edits; `mode="filtered"` + `nodeNames` reads one heavy node (e.g. long Code source) without pulling the whole workflow, which can truncate client-side.
- `n8n_list_workflows`; list/filter (search before duplicating logic).
- `n8n_delete_workflow`, `n8n_workflow_versions` (history/rollback), `n8n_instances` (multi-instance accounts only: list/switch the target instance; see `n8n-multi-instance`), `n8n_health_check` (returns the resolved `instanceName`).
- `n8n_test_workflow`; runs real nodes (Code, HTTP, DB writes, sends all fire). Ask the user before running when side effects exist.
- `n8n_executions`; list/inspect executions. **There is no `execute_workflow` tool.**
- `n8n_evaluations`; read evaluation test runs (n8n ≥ 2.30): list runs, aggregated metrics, per-case results. Read-only; runs are started from the n8n editor, not the API; a 403 indicates denied access: inspect actual version, scopes and policy before proposing any credential replacement.
- `n8n_manage_datatable`; Data Table CRUD, filtering, dry-run.
- `n8n_manage_credentials`; credential CRUD + `getSchema` discovery.
- `n8n_audit_instance`; security audit (hardcoded secrets, unauthenticated webhooks, error-handling gaps).
> (`n8n-nodes-base.set`). Mixing them is a common, silent mistake; see `n8n-mcp-tools-expert`.
3. `get_node` before configuring any node; read the live schema, don't assume.
These are opinionated best practices, not laws. Disagree with a call? It's all markdown -