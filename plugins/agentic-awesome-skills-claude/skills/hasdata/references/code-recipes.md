# Code recipes: wiring HasData into your code
| 401 | Bad/missing key | Fix; don't retry |
## Python: minimal client
## Python: retry + bounded concurrency
The retry example is for idempotent synchronous extraction, not async job submission. It creates a client per worker rather than sharing a mutable Requests Session. Cap `workers` at your plan's concurrency; anything higher just generates `429`s.
## TypeScript: minimal client