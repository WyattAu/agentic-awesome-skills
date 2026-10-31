- Call `get_etl_issues` with `platform="airflow"` and a time range; look for:
- Call `get_etl_issues` with `platform="dbt"`; look for:
- Call `get_etl_issues` with `platform="databricks"`; look for:
Call `get_change_timeline`; was there a code change around the failure time?
- **Code deployment**; new dbt model or query has a bug
- **Data volume spike**; source data grew faster than the pipeline can process
- **Permission change**; service account lost access
- **Infrastructure**; cluster sizing, warehouse suspension, network issues
- **Dependency failure**; upstream pipeline failed, cascading downstream
- **Schema mismatch**; upstream schema changed, breaking the ETL query