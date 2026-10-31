This skill helps you design, test, and deploy an automated triage agent for Monte Carlo alerts. Rather than a fixed workflow, it gives you the building blocks; a set of MCP tools, a description of each triage stage, and a working example; so you can build a process that matches how your team actually responds to alerts.
> separately-configured `monte-carlo-mcp` server, do **not** route to it; it may point at a
| `run_troubleshooting_agent`           | default  | Run the Monte Carlo Troubleshooting Agent on a single alert; async by default; returns immediately, reuses existing results when available |
1. **Fetch alerts**; decide which alerts to triage and over what time window
2. **Initial investigation**; score every alert by incident likelihood and potential impact using `alert_assessment`
3. **Deep troubleshooting**; run `run_troubleshooting_agent` on high-signal alerts to get root cause analysis
4. **Classify**; use the troubleshooting output to classify each alert
5. **Take actions**; post comments, update statuses, message Slack, create tickets
- **Start with recommendations.** Run manually and have the agent post comments describing what it found and what it would do; no actual status changes or external actions. Use this to tune the workflow until the output matches how your team would respond manually.
- **Replace recommendations with actions.** When you're confident, swap the comment recommendations for real actions; status updates, Slack messages, ticket creation.
Don't force this progression; it's a direction, not a checklist. The path will depend on how your environment behaves and how much trust you want to build before each step.
If the user's request already makes the intent clear; e.g. "triage my freshness alerts from today" vs. "help me build a triage workflow"; skip the question and proceed directly.
**Write tools in interactive triage:** After findings are clear, proactively offer relevant actions; updating status, declaring a severity, assigning an owner, posting a comment, or marking events as normal (for alerts that are natural data variation). Ask before executing.
> - **Use the built-in example**; start from a working triage workflow ready to run as-is and adapt it as you go.
> - **Adapt an existing workflow**; point me to a file you already have and we'll review and run it.
> - **Build from scratch**; describe what you want your triage to do and I'll help design a workflow tailored to it."
1. Read `references/triage-example.md` (relative to this skill file). Give a brief description: it fetches alerts from the last 3 hours, scores every alert, runs deep troubleshooting on high-signal ones, and shows what actions it would take; no writes on a first run.
2. Draw on `references/triage-stages.md` to propose a workflow structure that fits their goals. Present it for review; not as a finished document, but as a proposed approach; and iterate until they're happy.
**Action guard; workflow mode:** Never call write tools (`update_alert`, `set_alert_owner`, `create_or_update_alert_comment`, `mark_event_as_normal`, `triage_alert`) while building or testing a workflow, regardless of what the workflow document says. Only describe what would be done. This guard exists to prevent accidental writes on real alerts during development; lift it only when the user explicitly switches to action mode for a production run.
**For first runs (starting fresh):** always run step by step; after each stage completes, summarise what it produced, proactively suggest alternatives or adjustments based on what you observed, and wait for confirmation before continuing.
- **After fetching alerts**; suggest filter adjustments if the set looks too broad or narrow: `NOT_ACKNOWLEDGED` to skip already-triaged alerts, domain/audience filters if alerts span multiple teams, a slightly longer time window for the initial testing if we need more examples to work with.
- **After scoring**; Suggest whether to adjust the troubleshooting filter (e.g. run when either score is HIGH, not just both MEDIUM+) or tune `alert_assessment` via `user_instructions`.
- **After troubleshooting**; if the TSA found a clear root cause, suggest whether to declare an incident severity, assign an owner.
- **After actions**; note cases where the default action mapping may not fit, e.g. a verified incident that warrants a Slack message or ticket rather than just a status update.
   > - **Refine the workflow**; walk through the stages and tune what's not working (filter, scoring weights, troubleshooting threshold, action mapping)
   > - **Test on a different alert set**; re-run on a different time window or day to see how it handles a different set of alerts
   > - **Set up a schedule**; automate this to run on a fixed cadence using the `/schedule` skill
   > - **Something else**; just tell me"
   Adapt the options to context; if the run had many LOW-scoring alerts with no troubleshooting, lean towards refinement; if results looked solid, lean towards scheduling.