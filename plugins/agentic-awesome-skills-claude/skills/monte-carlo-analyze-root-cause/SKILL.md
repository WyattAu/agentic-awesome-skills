This skill helps investigate data incidents; freshness delays, volume anomalies, schema changes, field metric drift, and ETL failures; by guiding the agent through a systematic investigation using Monte Carlo's MCP tools. It combines observability metadata with optional direct data querying to find the root cause.
- **Database MCP server** (Snowflake, BigQuery, Redshift, Databricks); enables direct SQL queries for deeper data investigation. Without this, the skill can still analyze using MC's metadata tools but cannot profile actual data.
- **GitHub MCP server**; enables searching for recent PRs that may have caused the issue. Without this, the skill falls back to MC's query change detection.
| `get_etl_issues` | ETL pipeline issues; pass `platform` ("airflow", "dbt", or "databricks") |
| `get_etl_jobs` | Find ETL jobs that write to specific tables; pass `platform` param |
| `alert_assessment` | Optional ~2-min triage of an incident; returns HIGH/MEDIUM/LOW confidence and impact. Useful when you want a quick read before deciding to escalate to TSA. |
| GitHub MCP | Search for recent PRs (alternative to MC's `get_github_prs`; useful if the account has no MC GitHub integration) |
1. **No incident UUID.** `run_troubleshooting_agent` requires a UUID. The no-incident intake path (`references/intake-no-incident.md`) does not feed TSA. If that path later identifies a matching alert, return to Step 1 with the alert's incident UUID; the authorization conditions in Step 1.5 apply.
- If status is `success` on the first call, you have results; fold them straight into Step 7's synthesis and continue Steps 2–6 to corroborate.
- If status is `failed`, note the error and continue with the manual investigation only; do not re-run automatically.
Tell the user what you started: "I've kicked off the Troubleshooting Agent on this incident; it usually finishes in 4–8 minutes. While it runs, I'll continue investigating manually so we have findings either way."
1. Call `get_asset_lineage(mcons=[table_mcon], direction="UPSTREAM")`; what feeds this table?
2. Call `get_asset_lineage(mcons=[table_mcon], direction="DOWNSTREAM")`; what does this table feed?
**Investigation direction:** Follow any direction the user already supplied; clarify only if essential scope is missing. They may already have a hunch ("I think it's the Airflow job" or "check if someone changed the SQL"). Follow their lead; don't run all investigation paths blindly. If they have no preference, proceed with the most likely path based on the issue type.
   - Check freshness: `get_table_freshness`; is the upstream table also stale?
   - Check size: `get_table_size_history`; did the upstream table's volume change?
3. Check what upstream field values correlate with the anomaly (if DB connector is available; see Step 5).
**TSA poll #1.** If you started TSA at Step 1.5 and it has not yet returned `success`, call `get_troubleshooting_agent_results(incident_id=...)` once here (~30s after Step 1.5). If status is `success`, hold the result for Step 7. If still `running`, keep going; you'll poll again before Step 7. Don't block on it.
**If no database MCP is available:** Tell the user: "I can't query the warehouse directly; for deeper data investigation, connect a database MCP server. I can still analyze using Monte Carlo's metadata and the tools available." Continue the investigation with MC tools only.
**TSA poll #2.** If you started TSA at Step 1.5 and don't yet have results, call `get_troubleshooting_agent_results(incident_id=...)` one more time (~60–90s after poll #1). Stop on `success` or `failed`; if still `running` after this poll, present the manual findings now and tell the user TSA is still working ("TSA is still running on this incident; I'll fold its findings in once it completes if you'd like, or you can ask me to check back in a minute").
1. **Root cause or hypothesis**; distinguish confirmed mechanisms from temporal correlation, and state remaining uncertainty
2. **Evidence chain**; which tools confirmed each piece of the story
3. **Impact**; what downstream tables/consumers are affected (from Step 2)
4. **Recommended fix**; specific action to resolve the issue
5. **Prevention**; suggest monitoring to catch this earlier next time
- **TSA succeeded and agrees with the manual investigation**; lead with the unified root cause; cite both TSA's evidence chain and the corroborating manual findings.
- **TSA succeeded and contradicts the manual investigation**; surface both. Show TSA's verdict, show what the manual investigation found, and explain the disagreement (e.g. "TSA blames the upstream Airflow job, but `get_table_freshness` on that table is healthy"). Ask the user which thread they want to pull on.
- **TSA succeeded with low-signal output** (e.g. "no clear root cause"); present the manual findings as primary; cite TSA as a corroborating null result.
- **TSA failed or timed out**; present the manual findings only; mention TSA's failure briefly so the user knows it was tried.
- **Honor explicit user opt-outs.** If the user says "skip TSA", "manual only", or similar, do not call `run_troubleshooting_agent` or `alert_assessment`; proceed with the manual investigation only.