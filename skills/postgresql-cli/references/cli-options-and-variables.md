| `-c command` / `--command=command` | Execute one or more SQL commands, then exit. Commands can be separated by semicolons. Each `-c` string is sent as a single request; multiple SQL commands within it execute in one transaction (unless explicit `BEGIN`/`COMMIT`). Cannot mix SQL and meta-commands in one `-c`. Returns 0 on success, 1 on error, 2 if connection fails. |
| `-q` / `--quiet` | Quiet mode; no startup message, no informational messages |
| `-S` / `--single-line` | Single-line mode. Newline terminates a query (as if semicolon). Not recommended; execution order can be unclear when mixing SQL and meta-commands on the same line. |
| `-t` / `--tuples-only` | Print rows only; no headers, footers |
| `-w` |; | Don't prompt for password |
| `-W` |; | Force password prompt |
psql variables can be interpolated into SQL statements using colon-prefixed syntax. This is **plain text substitution**, not parameterized queries; understand the differences to use it safely.
| `:{?varname}` | Variable existence test. Always expands; `TRUE` if defined, `FALSE` otherwise. Designed for use in `\if` conditions. | `\if :{?myvar}` → true if `myvar` was set |
| `\:varname` | Escaped colon. The colon is not treated as a variable reference; `:varname` is passed through literally. | `SELECT \:not_a_var FROM t;` → `SELECT :not_a_var FROM t;` |
Note: Variable interpolation does NOT occur inside quoted SQL literals or identifiers. Therefore `':foo'` does not produce a quoted string from the variable (and wouldn't be safe even if it did; it can't handle embedded quotes properly). Use `:'foo'` instead.
**ON_ERROR_STOP**; The most important variable for scripts. Set it in scripts to ensure they abort on the first error instead of continuing:
**AUTOCOMMIT**; Turn off to manually manage transactions:
**FETCH_COUNT**; For very large result sets that would consume too much memory:
**ERROR / SQLSTATE / LAST_ERROR_MESSAGE**; For conditional logic:
| `PGPASSWORD` | Password (not recommended; use `~/.pgpass` instead) |
- `~/.psqlrc-18`; applies for psql 18.x
- `~/.psqlrc-18.3`; applies specifically for psql 18.3