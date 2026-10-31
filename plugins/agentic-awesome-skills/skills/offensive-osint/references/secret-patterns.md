## 17. Secret-Pattern Catalog: 48 candidate patterns (29 base + 19 modern)
- Patterns 22 (JWT), 23 (Bearer), 29 (Generic) trigger on test/example data frequently. Always look at *context*; a JWT in a `README.md` example block ≠ a JWT in a production `.env` file.
- Pattern 24 (Basic auth in URL) catches monitoring-tool URLs and CI-debug URLs as well as real creds; verify before alerting.