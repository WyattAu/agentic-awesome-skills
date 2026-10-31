These are referenced from the main SKILL.md; consult the relevant section when
## Workflow 1: Table health check: when opening or editing a model
- **Alerts**: any active/unacknowledged incidents; lead with these if present
- **Risk signals** (lite): flag if importance score is high, if key assets are downstream, or if alerts are already firing; these indicate the table warrants extra care before modification
> "The table `orders_status` was last updated 2 hours ago with 142K rows. It has 3 downstream dependents including `order_status_snapshot` (key asset). There are 2 active freshness alerts; this table warrants extra care before modification. Want me to run a full change impact assessment?"
**Auto-escalation rule; after completing steps 1–4 above:**
   "⚠️ <table_name> has <N> active alerts; your new model will inherit this data quality issue"
Skip getAssetLineage for new models; they have no downstream dependents yet.
## Workflow 2: Add a monitor: when new transformation logic is added
> **For detailed monitor creation guidance**; including parameter validation, field-type compatibility checks, and common error prevention; see the `monitor-creation` skill (`skills/monitor-creation/SKILL.md`). The workflow below is a quick-path for the common "just added a column, offer a monitor" case within a prevent session.
**Important; YAML format for `monitors apply`:**
All `create*MonitorMac` tools return YAML that is not directly compatible with `montecarlo monitors apply`. Reformat the output into a standalone monitor file with `montecarlo:` as the root key. The second-level key matches the monitor type: `custom_sql:`, `validation:`, `metric:`, or `comparison:`. The example below shows `custom_sql:`; substitute the appropriate key for other monitor types.
## Workflow 3: Alert triage: when investigating an active incident
- `updateAlert(alert_id="<id>", status="ACKNOWLEDGED")`; acknowledge it
- `setAlertOwner(alert_id="<id>", owner="<email>")`; assign ownership
- `createOrUpdateAlertComment(alert_id="<id>", comment="<text>")`; add context
## Workflow 4: Change impact assessment: REQUIRED before modifying a model
**Trigger:** Any expressed intent to add, rename, drop, or change a column, join, filter, or model logic. Run this immediately; before writing any code; even if the user hasn't asked for it.
before writing any code; even if the change seems small or safe.
- Deduplicate downstream dependents; if two changed tables share a downstream
  - <monitor name> watches <metric>; will be affected by this change
    I'll suggest a monitor for the new logic; or say 'add a monitor'
  → Explain: "There are N active alerts on this table; making this change now
  → Explain: "X downstream key assets depend on this table; I'd recommend
  → Say explicitly: "This adds new output logic; would you like me
  → Explain: "This table has [N] reads/day; a column rename without a
    2. If yes; treat as a breaking change. Recommend a
    3. If truly internal and never surfaces in output; confirm
    surfaces in the final SELECT it is a public output column -
## Workflow 5: Change validation queries: after a code change is made
**Required session context; do not activate without both:**
Using the context already in the session; the Workflow 4 findings, the file diff, and the `getTable` result; generate 3–5 targeted SQL validation queries that directly test whether this specific change behaved as intended.
These are not generic templates. Use the semantic meaning of the change from Workflow 4 context: which columns changed and why, what business logic was affected, what downstream models depend on this table, and what monitors exist. A null check on a new `days_since_contract_start` column should verify it is never negative and never null for rows with a `contract_start_date`; not just check for nulls generically.
### Step 1: Identify the change type from session context
From Workflow 4 findings and the file diff, classify the primary change. A change may span multiple types; classify the dominant one and note secondaries:
- **New column**; a new output column was added to the SELECT
- **Filter change**; a WHERE clause, IN-list, or CASE condition was modified
- **Join change**; a JOIN condition or join target was modified
- **Column rename or drop**; an existing output column was renamed or removed
- **Parameter change**; a hardcoded threshold, constant, or numeric value was changed
- **New model**; the file was newly created, no production baseline exists
### Step 2: Determine warehouse context from Workflow 4
- **Fully qualified table name**; e.g. `analytics.prod_internal_bi.client_hub_master`
- **Warehouse type**; Snowflake, BigQuery, Redshift, Databricks
- **Schema**; already resolved, do not re-derive
### Step 3: Apply database targeting rules (mandatory)
These rules are not negotiable; violating them produces queries that will fail at runtime:
### Step 4: Generate targeted validation queries
Include a row count comparison when a production baseline exists; for new models, include a dev-only count and expected-range check; it's the baseline signal that something unexpected happened.
Then generate change-specific queries based on what needs to be validated for this change type. Use the exact conditions, column names, and business logic from the diff and Workflow 4 findings; not generic placeholders. The goal for each change type:
**Filter change:** Verify that only the intended rows were reclassified; generate a before/after count showing how many rows were added or removed by the new condition using the exact filter logic from the diff, and a sample of the rows that changed classification. The sample helps the engineer confirm the right records moved.
**Join change:** Verify that the join didn't introduce duplicates; a uniqueness check on the join key is essential. Also verify row count didn't change unexpectedly. Query dev for uniqueness, both databases for row count.
**Column rename or drop:** Verify the old column name is absent and the new column (if renamed) is present in the dev schema. Also verify that downstream models referencing the old column name are identified; use the local ref() grep results from Workflow 4 if available.
**Parameter or threshold change:** Verify the distribution of values affected by the change; how many rows moved above or below the new threshold, and whether the count matches the engineer's expectation. Query both databases to compare before and after.
### Step 5: Add change-specific context to each query
This is what differentiates these queries from generic validation; the comment tells the engineer exactly what pass and fail look like for their specific change.
### Step 6: Save to local file
- Does not trigger automatically; only on explicit engineer request