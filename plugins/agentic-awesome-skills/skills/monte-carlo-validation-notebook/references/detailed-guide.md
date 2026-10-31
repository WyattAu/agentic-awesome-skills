5. Filter the changed files list to only `.sql` files under `models/` or `snapshots/` directories (at any depth; e.g., `models/`, `analytics/models/`, `dbt/models/`). These are the dbt models to analyze. If no model SQL files were changed, report that and stop.
7. **Fetch dbt_project.yml** for schema resolution. Detect the dbt project root by looking at the changed file paths; find the common parent directory that contains `dbt_project.yml`. Try these paths in order until one succeeds:
5. Filter to only `.sql` files under `models/` or `snapshots/` directories (at any depth; e.g., `models/`, `analytics/models/`, `dbt/models/`). If no model SQL files were changed, report that and stop.
   ⚠️ <total_count> models changed; generating validation queries for the first 10 only.
**Trigger:** Changed fields found in Phase 2b. **Exclude added columns** (from "New columns" in Phase 2b); only include fields that exist in prod.
    <Pattern Name (human-readable, e.g. "Total Row Count"; do NOT include pattern numbers like "Pattern 7:")>
2. Parameter cells (dev_db only; no prod_db if all models are new)
Focus on: how to install, configure connections, and run MC Bridge. Don't dump the entire README; extract just the setup-relevant sections.