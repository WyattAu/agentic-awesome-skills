Call `get_query_changes`; did the ETL query change around the time of the volume shift?
- Call `get_table_size_history`; did the source data volume also change?
- **Source data volume change**; upstream system sent more/fewer records than usual
- **Filter change**; ETL WHERE clause was modified, including/excluding different rows
- **Dedup logic change**; deduplication rules changed, producing more or fewer unique rows
- **Late-arriving data**; backfill or reprocessing loaded historical data
- **Partition swap**; a full partition was replaced with different data
- **Schema migration**; table was truncated and reloaded as part of a migration