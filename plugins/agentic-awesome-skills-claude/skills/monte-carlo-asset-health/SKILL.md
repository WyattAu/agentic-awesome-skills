> separately-configured `monte-carlo-mcp` server, do **not** route to it; it may point at a
1. `references/workflows.md` (relative to this file); exact tool calls, phases, and execution order
2. `references/parameters.md` (relative to this file); parameter conventions and field details
always appear with their heading.** Never omit a section; if there is no data,
hardcode or guess this URL; it varies by environment.
- raw_orders; FRESHNESS alert: not updated in 8h
- raw_payments; healthy
- dim_customers; healthy
- Investigate upstream raw_orders freshness; likely root cause of this table's staleness
### Metric definitions: exact data sources
- **🔴 Unhealthy:** Any active alerts on the asset (from `get_alerts` with statuses `["NOT_ACKNOWLEDGED", "ACKNOWLEDGED", "WORK_IN_PROGRESS"]`; see `parameters.md`)
- Do NOT recommend specific monitor types; that is outside this skill's scope