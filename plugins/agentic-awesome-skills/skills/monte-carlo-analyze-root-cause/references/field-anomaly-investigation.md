Call `get_query_changes`; did the ETL query modify how this field is computed?
- Call `get_table_freshness`; is the upstream data fresh?
- Call `get_table_size_history`; did upstream volume change?
If one dimension value has a much higher "bad rate" than others, it is a useful localization hypothesis, not proof of causation; e.g., "all rows from source_system='legacy_api' have NULL revenue, but rows from other sources are fine."
- **Upstream data quality issue**; bad data in source propagated downstream
- **ETL logic change**; CASE/COALESCE/type handling modified
- **New data source**; a new upstream source introduced unexpected values
- **Schema change**; column type changed, causing implicit conversions
- **Backfill**; historical data reprocessed with different logic
- **Null propagation**; upstream NULL values cascading through JOINs