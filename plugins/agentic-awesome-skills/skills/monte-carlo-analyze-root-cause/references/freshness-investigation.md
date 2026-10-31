Call `get_table_freshness` with the table's `full_table_id` and `resource_id`. Also call `get_table` with the same `full_table_id`; its response includes the table's `mcon`, which the lineage and query tools below need. Check:
- Call `get_etl_issues` with `platform="airflow"` and a time range; look for task failures, retries, or SLA misses
- Call `get_etl_issues` with `platform="dbt"`; look for compilation errors, test failures, or timeouts
- Call `get_etl_issues` with `platform="databricks"`; look for notebook failures, cluster issues
Call `get_query_changes`; did someone modify the ETL query recently?
- **Pipeline scheduling failure**; cron job stopped, DAG was paused, permissions revoked
- **Upstream freshness cascade**; an upstream table is stale, blocking this table's refresh
- **Query timeout**; the refresh query is taking too long and timing out
- **Resource contention**; warehouse is overloaded, queries are queued
- **Permission change**; service account lost access to source data