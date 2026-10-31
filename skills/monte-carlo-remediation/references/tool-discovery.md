- `mcp__airflow__trigger_dag_run`; an Airflow MCP tool
- `mcp__dbt_cloud__trigger_run`; a dbt Cloud MCP tool
- `mcp__github__create_pull_request`; a GitHub MCP tool
Monte Carlo's own tools are bundled by this plugin and namespaced under the plugin server; see the **Monte Carlo tool routing** block at the top of the skill. Examples:
If neither an MCP server nor a CLI tool is available for a service, you can often call its REST API directly using `curl`. This is the most flexible option; any service with an API is reachable.
**Note:** API calls require credentials. If you don't have the right tokens or environment variables, ask the user to configure secure credentials locally; never request tokens in chat.
| `*airflow*` | Pipeline orchestration; trigger DAG runs, retry failed tasks |
| `*dagster*` | Pipeline orchestration; launch runs, check status |
| `*prefect*` | Pipeline orchestration; create flow runs, check status |
| `*dbt*` | dbt operations; trigger job runs, get status, list jobs |
| `*github*` | Code changes; create PRs, issues, branches |
| `*gitlab*` | Code changes; create merge requests, issues |
| `*snowflake*` | Warehouse access; execute queries, get table info |
| `*bigquery*` | Warehouse access; execute queries, get table info |
| `*databricks*` | Warehouse access + orchestration; queries, trigger jobs |
| `*fivetran*` | Ingestion; trigger connector sync, check status |
| `*jira*` | Issue tracking; create issues, update status |
- `gh`; covers GitHub operations (PRs, issues, API calls) without needing a GitHub MCP
- `dbt`; covers dbt operations (run, test, retry) without needing a dbt Cloud MCP
- `git`; check availability for code changes
- `curl`; check availability for calling any REST API
1. **Can I investigate?**; Monte Carlo MCP is always needed. If it's not connected, you cannot proceed.
2. **Can I execute the fix?**; Check MCP servers first, then CLI tools, then API access.
> - ✅ Create a code fix (`gh` CLI available; no GitHub MCP needed)
The `gh` CLI is often the most practical option; it doesn't require a GitHub MCP server and supports the full GitHub API via `gh api`.