# psql Meta-Commands: Core Reference
psql maintains an internal **query buffer**; a working area where SQL commands are assembled before being sent to the server. Understanding how the buffer works clarifies the behavior of many meta-commands:
- **`\e` / `\edit`** opens the buffer in an external editor; when the editor closes, the modified content is re-parsed; complete queries (those ending with `;`) are executed immediately, and any remaining text stays in the buffer.
- **Single quotes**: `'hello world'`; the argument is `hello world`. Single quotes prevent variable interpolation and backquote expansion.
- **Double quotes**: `"hello world"`; the argument is `hello world`. Variable interpolation (`:varname`) IS performed inside double quotes, but backquote expansion is NOT.
- A `\\` (double backslash) anywhere in the argument text causes psql to stop parsing at that point; everything before `\\` is the argument, everything after is ignored. This is useful for adding inline comments:
With no argument, escapes to a sub-shell (psql resumes when sub-shell exits). With an argument, executes the shell command. The entire remainder of the line is taken as the command; no variable interpolation or backquote expansion.
**Note**: The file or command is written to only if the query successfully returns zero or more tuples; not if the query fails or is a non-data-returning SQL command. This means even an empty result set (0 rows) will trigger output.
Executes the current query, then treats each column of each row as a SQL statement to execute. NULL fields are ignored. Generated queries are sent literally; no psql meta-commands or variable references. Execution continues on error unless `ON_ERROR_STOP` is set. Setting `ECHO` to `all` or `queries` is recommended when using `\gexec` to see what's being executed.
- `colV`; vertical header (default: column 1)
- `colH`; horizontal header (default: column 2, must differ from colV)
- `colD`; data displayed in the grid (default: the remaining column)
- `sortcolH`; optional sort column for horizontal header (must be integers)
**stdin behavior**: When using `\i -`, psql reads from standard input until an EOF indication or `\q` meta-command. This can be used to intersperse interactive input with input from files. Note that Readline editing is only available at the outermost level; it is not active when reading from a nested file.
**What gets redirected**: "Query results" includes tables, command responses, notices, and output from `\d` commands; but **not error messages**. Error messages always go to stderr.
- `commands` (default); backslash commands
- `options`; command-line options
- `variables`; configuration variables
Unlike most meta-commands, the entire line is the argument; no variable interpolation.