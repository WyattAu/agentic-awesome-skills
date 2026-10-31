# redis-cli: Redis Command Line Interface
- **Never pass passwords via `-a` in production**; visible in shell history and process listings. Use `REDISCLI_AUTH` environment variable instead.
- **`KEYS *` blocks the server** on large databases; always use `SCAN` in production code.
- **`MONITOR` logs all commands** including sensitive data; use cautiously, and never for extended periods on production servers.
- **`FLUSHALL` / `FLUSHDB` are irreversible**; verify target database with `CLIENT LIST` or `INFO keyspace` first.
EXISTS key [key ...]           # Check existence (O(N) for multi); returns count
SCAN guarantees: a full iteration (cursor 0 → cursor 0) always returns all elements that existed for the entire duration. Elements may appear multiple times; handle duplicates in your application.
# Raw output (no type prefixes): default when piping
| `references/module-data-types.md` | Module data types: JSON (RedisJSON), Vector Sets (Redis 8.0+), Bloom Filter, Cuckoo Filter, Top-K, Count-Min Sketch, T-Digest, TimeSeries (TS.*), Full-Text Search / RediSearch (FT.*); with full command syntax and behavioral notes | Working with Redis module data types, similarity search, probabilistic data structures, time series data, full-text search |