All metric queries use the same `timeWindow` constant (`14d`); defined as `TIME_WINDOW` in [lib/queries.mjs](../lib/queries.mjs) and covered by the repo test suite. Mixing windows silently produces incompatible rollups; never pin a per-query `since`.
| CLI version | `vercel --version` | Everything | Exit with "upgrade to v53+"; v53 is the skill's compatibility floor |
| `metrics.cwvLcpByRoute`, `cwvInpByRoute`, `cwvClsByRoute`, `cwvTtfbByRoute`, `cwvCount`, `cwvCountByRoute` | `vercel metrics vercel.speed_insights_metric.*` (`p75` for vitals, `sum` for counts) `--since 14d` | `cwv_poor` gate | Empty when no Speed Insights measurements are returned for the 14-day window; gate stays dormant; do not infer disabled vs no traffic unless another signal proves it |
`lib/vercel.mjs`'s `runVercelJson()` parses stdout as JSON first (the most reliable signal; the CLI emits structured error payloads even when exit code is non-zero), and only falls back to stderr substring matching when JSON parsing fails:
| `FORBIDDEN` | 403; role lacks permission | Skip that endpoint; continue with degraded signal; surface in report |
Field naming rule: the metric ID's dots become underscores, and the aggregation suffix is appended; `vercel.request.count` + `sum` → `vercel_request_count_sum`. `lib/vercel.mjs::normalizeSummary()` flattens `summary[]` into `[{<dim>: v, ..., value: <n>}]`.
Array of `{id, description}` entries; NOT an object. Many metric IDs in earlier docs don't exist: there is no `vercel.function.cold_starts`, no `vercel.cache.hits`. Cache state is the `cache_result` dimension on `vercel.request.count`.
- `resourceConfig.fluid` (boolean); **Fluid Compute toggle**
- `defaultResourceConfig.fluid`; template for new functions
- `security.botIdEnabled` (boolean); **BotID toggle**
- `security.managedRules.bot_filter` (`{active, action}`); firewall rule
- `webAnalytics` (`{id}`); installed but `features.webAnalytics` says enabled state
May return `Error: Costs not found (404)`. Treat that queried error as `USAGE_UNAVAILABLE` and degrade; the skill can still produce a useful report from metrics + scanner. Do not use this explanation when `usageError` is `NOT_COLLECTED_OBSERVABILITY_BLOCKED` or another `NOT_COLLECTED_*` value; those mean the audit stopped before `vercel usage` ran.
CLI error message strings are not stable contracts; they can change between versions. Detecting `OPLUS_REQUIRED` by greping `stderr.includes('Observability Plus')` will break the moment Vercel rewords the message.