# unslop: Strip AI Writing Patterns via CLI
The `--deterministic` flag makes output reproducible; same input always produces same output. The `--stdin` flag reads from stdin, enabling shell pipeline composition.
- ❌ Don't run on code files; unslop targets prose, not source code
- Processes prose only; not code, JSON, or structured data
- unslop reads from stdin and writes to stdout; no file system side effects by default