# psql Meta-Commands: Object Inspection (\d family)
- `+`; extra info (size, description, ownership)
- `S`; include system objects
- `x`; expanded display (must follow `S` or `+`, NOT immediately after `\d`)
`\dE` / `\di` / `\dm` / `\ds` / `\dt` / `\dv`; List foreign tables, indexes, materialized views, sequences, tables, or views. Combine letters: `\dti` lists both tables and indexes.
| `\dO[Sx+] [pattern]` | Collations (only collations usable with current database encoding; results vary by database) |