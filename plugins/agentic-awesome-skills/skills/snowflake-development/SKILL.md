- Use `snake_case` for all identifiers. Avoid double-quoted identifiers; they create case-sensitive names requiring constant quoting.
- Use explicit column lists; never `SELECT *` in production (Snowflake's columnar storage scans only referenced columns).
### Stored Procedures: Colon Prefix Rule
- `SELECT *` breaks on schema changes; use explicit column lists.
-- Tasks start SUSPENDED; you MUST resume them
### TO_FILE: Common Error Source
- Do NOT include empty/null values in edit specs; clears existing values.
- Never modify production agents directly; clone first.
- DataFrames are lazy; executed on `collect()`/`show()`.
- Do NOT use `collect()` on large DataFrames; process server-side.