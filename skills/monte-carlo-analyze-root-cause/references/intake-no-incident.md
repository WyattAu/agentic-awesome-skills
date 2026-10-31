- If a matching alert exists, use its details to drive the investigation; proceed as if the user provided an incident ID
- **Freshness:** `get_table_freshness`; when was it last updated? Is it overdue?
- **Volume:** `get_table_size_history`; has the row count changed unexpectedly?
- **Schema:** `get_table(mcon=..., include_fields=true)`; check current schema
- **Query activity:** `get_queries_for_table`; are write queries still running?
| Problem is about an AI agent (quality, latency, traces, conversations) | Agent issue | Hand off; read and follow `../../troubleshoot-agent-traces/SKILL.md` |
> **TSA note.** This intake path intentionally does **not** invoke the Troubleshooting Agent (TSA), because `run_troubleshooting_agent` requires a Monte Carlo incident UUID and this path starts without one. If Step 3 above identifies a matching alert, treat the user as having provided that alert's incident ID and re-enter the main `SKILL.md` flow at Step 1; Step 1.5 there checks authorization before any new paid TSA run. If no matching alert is found, run the manual investigation only.