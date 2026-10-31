Each stage of a triage workflow is optional and customisable. Design your workflow around the stages that match how your team manually reviews alerts; automate the parts that are repetitive or time-consuming, and keep humans in the loop for the parts that need judgement.
Collect the alerts you want to triage. `get_alerts` supports the following filters; combine them to define your triage scope:
- `statuses`; filter by alert status. Pass `NOT_ACKNOWLEDGED` to only pick up alerts that haven't been triaged yet. Other values: `ACKNOWLEDGED`, `WORK_IN_PROGRESS`, `FIXED`, `EXPECTED`, `NO_ACTION_NEEDED`.
- `domain_ids`; limit to one or more Monte Carlo domains. Use `getDomains` to look up IDs. Useful if different teams own different parts of the data estate.
- `alert_types`; filter by alert category.
- `priorities`; `P1` through `P5`.
- `table_mcons`, `table_names`, `table_schemas`, `table_databases`; narrow triage to specific tables or parts of the warehouse.
- `first`; number of alerts per page (max 100, default 20). Check for a `truncation_note` in the response; if present, paginate using `cursor` to retrieve the remaining alerts.
Take care to avoid triaging too many alerts in one batch; where required, split alerts across multiple triage runs.
This stage replicates what a knowledgeable engineer does when scanning the alert feed; quickly assessing what's fired and how serious it looks. `alert_assessment` is lightweight enough to run on every alert.
- **`incident_likelihood`** (HIGH/MEDIUM/LOW); how likely the alert represents a real issue. Affected by: number of events, presence of concerning root causes (query changes, failures), how much thresholds were exceeded, and how noisy the monitor typically is.
- **`alert_impact`** (HIGH/MEDIUM/LOW); how significant the potential downstream impact is.  Use cases impacted.  Dashboards affected etc.
- **`alert_description`**; plain-language description of what happened in the incident.
- **`triage_summary`**; the key reasoning behind the incident likelihood and potential impact scores.
`alert_assessment` is **read-only**; it returns a verdict but records nothing. That's exactly what you want for this stage: score every alert cheaply, then decide per alert whether it warrants troubleshooting and which action to take. The workflow persists outcomes later, in Stage 5, through the explicit action tools; so you stay in control of what gets written and when.
`triage_alert` is the **persisted** counterpart: it scores *and* writes the verdict back onto the alert (marks it triaged, posts a notification, records ML feedback), identical to the in-app Triage button. It's the right tool for interactive "triage this alert" requests where the user wants the result recorded, but it's a poor fit for the scoring stage of a batch workflow; it would mark every alert in the batch triaged and fire a notification for each. Keep batch scoring on `alert_assessment`, and reserve `triage_alert` for the case where recording a single alert's triage *is* the action. It's idempotent (re-running on an already-triaged alert returns the existing verdict) and requires the `mcp/edit` scope.
`run_troubleshooting_agent` runs the Monte Carlo Troubleshooting Agent on a single alert. This is substantially more expensive than `alert_assessment`; it tracks the issue upstream through lineage, analyses all queries involved, examines relevant PRs, and samples affected tables to identify root cause.
You can adjust this threshold based on your environment; for example, also running troubleshooting when either score is HIGH (even if the other is LOW), while still requiring MEDIUM/MEDIUM as the baseline.
- `success`; a previous analysis already completed; results are available immediately, no polling needed
- `queued`; the job was accepted but hasn't started yet; wait ~30 seconds then start polling
- `running`; the job is in progress; poll with increasing intervals (30s, 60s, 60s…)
These categories are a starting point. Adapt them to the language your team uses; if you have an internal classification scheme, map to that instead.
`create_or_update_alert_comment`; always a good starting point. Comments provide a record of what the agent found and recommended, as an externally visible write that may notify people. Useful at every stage, regardless of whether you automate anything else.
- **Troubleshot alerts**: 2–4 sentences; classification, reasoning, action taken or recommended
`update_alert`; set status based on classification:
- **Mark events as normal**; for alerts classified as natural variation, marking the underlying events as normal allows the detector to adapt thresholds to prevent further alerts on similar patterns. Only applies to monitors using automated (ML-based) thresholds; has no effect on static-threshold monitors.
- **Declare an incident** (`update_alert` with `declared_incident_severity`); promotes the alert to an incident, escalating visibility. Values: `SEV_1`–`SEV_4`. Use `NO_SEVERITY` to clear. Appropriate for verified ongoing incidents.
- **Assign ownership** (`set_alert_owner`); route a confirmed incident or required investigation to the right person.
- **Slack**; message a channel or individual with a triage summary or escalation
- **Linear / Jira / Teams**; create a ticket for confirmed incidents