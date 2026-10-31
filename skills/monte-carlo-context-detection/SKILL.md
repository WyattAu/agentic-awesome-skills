> separately-configured `monte-carlo-mcp` server, do **not** route to it; it may point at a
- The user's message relates to data quality, alerts, incidents, coverage, or Monte Carlo; but doesn't clearly match a single skill in the routing table
- A skill or workflow is already active in the conversation; the active skill owns the conversation, do not intercept
- The user's message clearly matches a single skill in the CLAUDE.md routing table; route directly, no need for context detection
- The user is editing a dbt model; defer to the `prevent` skill only when its hooks are verified active
This skill is purely reactive; it activates for ambiguous or multi-step data-related messages and routes them.
Before doing anything else, check whether the user's message unambiguously matches a single existing skill. If so, **skip the rest of this workflow** and immediately load that skill; do NOT read `references/signal-definitions.md`, do NOT make API probes.
**Always scope MCP calls tightly.** Unscoped `get_alerts`, `search`, or `get_monitors` on large accounts can return hundreds of results, overflow the tool-result token limit, spill to disk, and force expensive chunk reads; burning user tokens and risking workflow failure. Minimum scoping:
> 1. **Investigate alerts**; triage and fix active data issues (incident response workflow)
> 2. **Improve monitoring**; find coverage gaps and create monitors (proactive monitoring workflow)