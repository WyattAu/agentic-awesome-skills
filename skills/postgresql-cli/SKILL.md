# psql: PostgreSQL Interactive Terminal
psql is PostgreSQL's feature-rich interactive terminal. It lets you write and execute queries, inspect database objects, import/export data, script batch operations, and customize output formatting; all from the command line.
# Windows: install PostgreSQL via the official installer or use WSL
psql ships as part of the `postgresql-client` package. The server (`postgresql`) is not required; you only need the client to connect to a remote PostgreSQL instance.
**Connection precedence**: CLI flags > environment variables > `pg_service.conf` > defaults. **Password precedence**: connection string/password flag > `PGPASSWORD` env > `~/.pgpass`. Use `~/.pgpass` instead of `PGPASSWORD` in production; `PGPASSWORD` is visible in process listings (`ps aux`).
- `+`; extra info (size, description): `\dt+`, `\l+`, `\du+`
- `S`; include system objects: `\dtS`, `\dfS+`
- `x`; expanded display mode: `\dt+x` (note: `\dx` is a different command; `x` must follow `S` or `+`)
-- Client-side (runs with client permissions, no superuser needed); preferred
`\copy` is the go-to for day-to-day work; it uses the client's filesystem and permissions, not the server's.
Variables in SQL: `:'varname'` (quoted string value, escapes embedded quotes), `:"varname"` (double-quoted identifier), `:'varname'::type` (with cast), `:varname` (unquoted; can break SQL), `:{?varname}` (tests existence, expands to TRUE/FALSE).
- `\startpipeline`; begin pipeline block
- `\endpipeline`; end pipeline block and process remaining results
- `\sendpipeline`; append current query buffer to pipeline without waiting
- `\syncpipeline`; send sync message without ending pipeline
- `\flushrequest`; request server flush without sync
- `\flush`; manually push unsent data to server
- `\getresults [N]`; read pending results (N=0 or omitted means all)
- `interval`; seconds between executions (default: 2, overridable via `WATCH_INTERVAL` variable)
- `count`; stop after N executions
- `min_rows`; stop if query returns fewer than N rows
The `:'varname'` form (quoted) is always safer than `:varname` (unquoted), because unquoted substitution can break SQL syntax or enable injection. Use `:"varname"` for identifiers (table/column names); it properly escapes embedded double quotes.
- **`references/meta-commands-core.md`**; Core meta-commands: query buffer behavior, argument parsing rules, connection management, query execution, `\copy` syntax, and scripting commands (`\if`, `\i`, `\o`, backquote expansion). Read this when you need exact syntax or behavioral details for any backslash command.
- **`references/meta-commands-inspection.md`**; Full `\d` command reference: all object inspection commands, modifiers (`S`, `+`, `x`), and pattern matching rules. Read this when exploring database schema or when the user needs to inspect tables, indexes, views, functions, privileges, etc.
- **`references/meta-commands-formatting.md`**; Output formatting (`\pset` options and all format descriptions), pipeline mode, `\watch`, `\crosstabview`, and session management (`\e`, `\ef`, `\ev`, `\timing`, etc.). Read this when the user needs to control output format or use pipeline mode.
- **`references/cli-options-and-variables.md`**; All CLI flags, environment variables, psql internal variables (AUTOCOMMIT, ON_ERROR_STOP, ECHO, FETCH_COUNT, etc.), prompt customization, `~/.psqlrc` configuration, and SQL interpolation syntax. Read this when configuring psql startup behavior, writing scripts that depend on variable state, or customizing prompts.
- **`references/tips-workflows.md`**; Practical workflows (exploring a new database, understanding table structure), scripting patterns (safe scripts, conditional execution, `\gexec`), output control for automation, and data import/export patterns. Read this when the user asks how to accomplish a specific task with psql.
- **`references/tips-advanced.md`**; Performance tips, debugging/introspection (`EXPLAIN`, lock analysis, `ECHO_HIDDEN`), safety best practices (ON_ERROR_STOP, transaction patterns, search_path safety), and common gotchas. Read this for lock analysis, query plan inspection, and troubleshooting.
This distinction matters when writing scripts that rely on comment behavior; only SQL-standard comments are stripped client-side.