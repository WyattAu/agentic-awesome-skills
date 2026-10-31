Compare against the alert details; what columns were added/removed/modified?
Call `get_query_changes`; schema changes may come from ETL, DDL, source evolution, or connector inference:
- Call `get_etl_issues` with `platform="dbt"`; dbt model changes are the most common source
- Call `get_etl_issues` with `platform="airflow"`; pipeline deployment may have changed the schema
- **dbt model change**; column added/removed in a model definition
- **Migration script**; ALTER TABLE or CREATE TABLE AS SELECT with new schema
- **Source schema change**; upstream system changed its schema, propagating downstream
- **Type promotion**; implicit type coercion changed (e.g., INT → FLOAT)
- **Column rename**; a column was renamed, breaking downstream references