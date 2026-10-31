The Monte Carlo MCP server (`monte-carlo-mcp`) provides the investigation tools used in the workflows below. The workflows reference key tools by name (e.g., `get_alerts`, `run_troubleshooting_agent`, `get_asset_lineage`), but **use any Monte Carlo tool that helps**; the server has additional tools beyond what the workflows explicitly call out. Explore what's available.
> **Note on tool call examples:** The code blocks below show key parameters to guide you. Always check the tool's own description for the complete parameter list and exact parameter names; they are authoritative.
Remediation actions are executed via whatever tools are available; MCP servers, CLI tools, or APIs. See Workflow 2 (Capability Discovery) and `references/tool-discovery.md` for how to detect and use them. Use whatever works; don't limit yourself to a prescribed list.
Before proposing ANY remediation action, you MUST complete this investigation. Do not skip steps; incomplete context leads to wrong fixes.
**While TSA runs, proceed with Steps 4–6 in parallel**; gather lineage, table context, and query data while waiting. Then poll for TSA results:
**When TSA succeeds, read both the `tldr` and the verifications section.** The `tldr` summarizes the root cause; this is your primary input for choosing a remediation action. The `full_response` includes a "verifications to confirm the root cause" section with specific checks (queries to run, things to compare, upstream systems to inspect). These verifications are often actionable remediation steps themselves; use them to guide what to do next or present them to the user as concrete next steps.
Note: `has_relationships=false` means no relationships were returned/tracked; it does not prove that real dependencies are absent.
The config contains the SQL query or validation conditions; this tells you exactly what the monitor checks, which is essential for understanding what went wrong and what the fix should be.
1. **MCP servers**; scan your tool list for `mcp__*__*` patterns (e.g., `mcp__airflow__trigger_dag_run`)
2. **CLI tools**; you have shell access; check for tools like `gh`, `dbt`, `airflow`, `curl` via `which <tool>`
3. **APIs**; any service with a REST API is reachable via `curl` if you have the right credentials
Don't assume any particular tool is available. But also don't assume MCP is the only option; a `gh pr create` via the CLI works just as well as a GitHub MCP tool.
1. **Always produce the remediation plan**; describe exactly what needs to happen, step by step
2. **Provide runnable commands**; give the user the exact commands they can run manually (e.g., `airflow dags trigger <dag_id>`, `dbt run --select <model>`)
3. **Present findings and ask for next steps**; tell the user what you found, what you recommend, and ask how they'd like to proceed
4. **Document on the alert**; use `create_or_update_alert_comment` to record the diagnosis and recommended fix
- **Explain before executing**; never take action without telling the user what and why
- **Confirm destructive operations**; wait for explicit user approval
- **Ask the user when uncertain**; don't guess at a fix
- **One action at a time**; execute one action, then decide next step
- **Log necessary redacted evidence**; document authorized actions on the alert via `create_or_update_alert_comment`
**Goal:** Close out the incident properly; update status, document, and prevent recurrence.
- `FIXED`; the root cause was identified and remediated
- `EXPECTED`; the alert fired on expected behavior (e.g., planned maintenance)
- `NO_ACTION_NEEDED`; the issue resolved itself or is not actionable
Do not automatically create monitors or tickets; suggest them and let the user decide.
- **NEVER skip the investigation phase.** A wrong diagnosis leads to a wrong fix; or worse, a fix that causes new problems.
- **NEVER assume external MCP tools are available.** Always check first. A missing tool is not an error; present findings to the user and ask for next steps.