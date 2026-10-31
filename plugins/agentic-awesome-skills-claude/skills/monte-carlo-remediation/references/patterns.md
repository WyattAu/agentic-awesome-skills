Common data quality issue patterns with example remediation workflows. These are illustrative examples for reasoning; not rigid step-by-step procedures. Real incidents often combine multiple patterns or present unique variations. Use these as a starting point, then adapt based on the specific TSA findings and available tools.
1. Check upstream lineage; is the source table also stale?
2. Check recent write queries; what pipeline usually updates this table?
- Re-check table freshness: `getTable(mcon="<table_mcon>")`; has `last_updated_on` moved forward?
- Re-check the alert: `getAlerts(alert_ids=["<alert_id>"])`; has it resolved?
- "Dependency failed; upstream model did not complete"
1. Read the TSA `full_response` carefully; it often contains the actual dbt error message.
3. If upstream models also have alerts, the root cause is likely further upstream; remediate that first.
- Re-check table: `getTable(mcon="<table_mcon>")`; has the model been rebuilt?
2. Check blast radius; who consumes this table?
Not every issue has a clear, automatable fix. When the root cause is unclear or complex, the best approach is to present full context to the user and ask for direction; not guess at a fix.