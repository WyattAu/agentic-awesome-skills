## `get_alerts`: use snake_case parameters
Pass ISO 8601 timestamps computed from the current date; e.g. for a 7-day
## `search`: finding the right table identifier
## `get_table`: table metadata and stats
- `importance_score`; float 0-1 (for Importance in header)
- `is_important`; boolean, true if key asset (for ⭐️ indicator)
- `table_stats.avg_reads_per_active_day`; average reads per active day
- `table_stats.avg_writes_per_active_day`; average writes per active day
## `get_monitors`: checking if monitors are paused
## `get_mc_webapp_url`: get Monte Carlo base URL
(e.g., `https://getmontecarlo.com`; the actual value depends on the customer's
Monte Carlo links; never hardcode the base URL:
## `get_asset_lineage`: direction and edge interpretation
before Phase 3 can run; do not skip pages.