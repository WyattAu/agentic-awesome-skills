| `eas observe:events` | Custom events emitted by the app via `logEvent`; name summary, all events, or filtered by event name (paginated) |
- `--platform ios` or `--platform android`; filter by platform (default: both)
- `--start <ISO date>` and `--end <ISO date>`; explicit time range
- `--days <N>`; show data from the last N days (mutually exclusive with `--start`/`--end`)
- `--project-id <id>`; run against a specific project without needing a project directory. When passed, the command will not try to create a new EAS project where one is unneeded.
- `--json`; machine-readable output (implies `--non-interactive`)
- `--non-interactive`; fail instead of prompting
# Multiple metrics: each renders as its own table
# Pagination: pass the endCursor from the previous run
- `--sort <oldest|newest|slowest|fastest>`; defaults to `oldest`
- `--limit <N>`; samples per page (default 10, max 100)
- `--after <cursor>`; pagination cursor from the previous run
- `--app-version <version>`; filter by app version string
- `--update-id <id>`; filter by EAS update ID
- Summary header shows the metric name, time range, and total sample count across all versions (e.g. `TTI samples for the last 60 days; 1,234 total events`)
# Pagination: each platform has its own cursor; pass the relevant endCursor
- `--metric <cold_ttr|warm_ttr|nav_tti>`; navigation metric(s) to display, can be repeated. Defaults to all three.
- `--stat <median|p90|count>`; statistic(s) per metric. Aliases: `med` → `median`, `event_count` / `eventCount` → `count`.
- `--limit <N>`; routes per page (default **50**, max **200**, different from `metrics`/`events` which default to 10).
- `--after <cursor>`; pagination cursor from the previous run.
- `--app-version <version>`; filter by app version string.
- `--build-number <number>`; filter by app build number (routes-only).
- `--route-name <name>`; filter by route name. Repeatable; only the listed routes are returned across both platforms. Duplicates are de-duplicated; omitting the flag returns all routes.
- `--update-id <id>`; filter by EAS update ID.
- `--all-events`; when no event name argument is given, list all events instead of the name summary. Cannot be combined with an event name argument.
- `--session-id <id>`; filter to events from a single session (events-only)
- `--app-version <version>`; filter by app version string
- `--update-id <id>`; filter by EAS update ID
- `--limit <N>`; events per page (default 10, max 100)
- `--after <cursor>`; pagination cursor
**Empty-result helper:** if a specific event name is queried and returns no events, the command prints a yellow `No events found matching "<name>"` warning followed by the available event names + counts in the same time range; useful for fixing typos.