EAS Observe collects app performance telemetry and custom events from Expo apps and exposes them through six EAS CLI commands. Pass the `--help` flag to any command for the latest API; the flags below were verified against `eas-cli` 21.8.0.
> Source: https://docs.expo.dev/eas/observe/eas-cli/; the canonical CLI page. This reference adds table layouts, JSON output shapes, and pagination details that the docs page does not cover.
| `eas observe:events` | Custom events emitted by the app via `logEvent`; name summary, all events, or filtered by event name (paginated) |
- `--start <ISO date>` and `--end <ISO date>`; explicit time range
- `--days <N>`; show data from the last N days (mutually exclusive with `--start`/`--end`, minimum 1)
- `--project-id <id>`; run against a specific project without needing a project directory. When passed, the command will not try to create a new EAS project where one is unneeded.
- `--json`; machine-readable output (implies `--non-interactive`)
- `--non-interactive`; fail instead of prompting
**Which command takes which alias.** `observe:metrics` (positional argument) and `observe:metrics-summary --metric` accept **all nine** aliases; startup and navigation. `observe:routes --metric` accepts only the three navigation aliases. Use the `nav_` prefix everywhere; there are no bare `cold_ttr` / `warm_ttr` aliases.
# Multiple metrics: each renders as its own table
**Stat flags:** exactly `min`, `median`, `max`, `average`, `p80`, `p90`, `p99`, `eventCount`. This command takes **no aliases**; `med`, `avg`, and `count` are rejected here (they work only on `observe:routes`).
# Pagination: pass the endCursor from the previous run
- `--sort <oldest|newest|slowest|fastest>`; defaults to `oldest`
- `--limit <N>`; samples per page (default 10, max 100)
- `--after <cursor>`; pagination cursor from the previous run
- `--app-version <version>`; filter by app version string
- `--update-id <id>`; filter by EAS update ID
- Summary header shows the metric name, time range, and total sample count across all versions (e.g. `TTI samples for the last 60 days; 1,234 total events`)
# Pagination: each platform has its own cursor; pass the relevant endCursor
- `--metric <nav_cold_ttr|nav_warm_ttr|nav_tti>`; navigation metric(s) to display, can be repeated. Defaults to all three.
- `--stat <median|p90|count>`; statistic(s) per metric. Aliases: `med` → `median`, `event_count` / `eventCount` → `count`.
- `--limit <N>`; routes per page (default **50**, max **200**, different from `metrics`/`events` which default to 10).
- `--after <cursor>`; pagination cursor from the previous run.
- `--app-version <version>`; filter by app version string.
- `--build-number <number>`; filter by app build number (routes-only).
- `--route-name <name>`; filter by route name. Repeatable; only the listed routes are returned across both platforms. Duplicates are de-duplicated; omitting the flag returns all routes.
- `--update-id <id>`; filter by EAS update ID.
- `--all-events`; when no event name argument is given, list all events instead of the name summary. Cannot be combined with an event name argument.
- `--session-id <id>`; filter to events from a single session (events-only). With no event name argument, this lists the session's events instead of the event-name summary. For the full timeline; metrics as well as log events; use `observe:session`.
- `--app-version <version>`; filter by app version string
- `--update-id <id>`; filter by EAS update ID
- `--limit <N>`; events per page (default 10, max 100)
- `--after <cursor>`; pagination cursor
**Empty-result helper:** if a specific event name is queried and returns no events, the command prints a yellow `No events found matching "<name>"` warning followed by the available event names + counts in the same time range; useful for fixing typos.
Shows the full timeline of metric and log events for a single session; the CLI equivalent of the dashboard's session view. Use it after `observe:metrics` surfaces a slow sample: take that sample's `sessionId` (present in `--json` output) and replay everything that session recorded.
- `[SESSIONID]`; positional. Omit it in interactive mode to choose from a list. **Required in non-interactive mode**, including under `--json`.
- `--event-name <name>`; metric or log event used to build the candidate session list (for example `tti`, `cold_launch`, `login_pressed`). Prompts when omitted in interactive mode.
- `--sort <slowest|fastest|newest|oldest>`; orders the candidate events. Prompts when omitted in interactive mode. No default, unlike `observe:metrics`.