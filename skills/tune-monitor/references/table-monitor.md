likely share a common cause; e.g., a delayed pipeline affects freshness across several tables,
of correlated alerts may share a real upstream incident, not harmless noise; address the root cause
A pair with 1-2 anomalies is not a clear noise pattern; it could be legitimate.
anomalies alone is not enough; the pattern must clearly indicate noise.
- Check the delta vs threshold in the incident; if deltas are consistently just above the auto
- Check "time since update" vs threshold in the incident; if the table regularly goes quiet
**Do NOT recommend changes for schema anomalies**; they are not tunable via the
Table monitor tuning uses **`create_or_update_table_monitor_asset_rule`**; **not**
  - Omit `schedule_type`; the platform's existing schedule is preserved.
  - `rule_type="total_row_count"`: pass `alert_conditions=[{"type": "lookback", "operator": "OUTSIDE_RANGE", "lower_threshold": <pct>, "upper_threshold": <pct>, "threshold_lookback_minutes": <minutes>}]`; thresholds are relative percentages (e.g. -50 / 50 = ±50 %).
  - `rule_type="total_row_count_last_changed_on"`: pass `alert_conditions=[{"type": "threshold", "operator": "GT", "threshold_value": <minutes>, "threshold_lookback_minutes": <minutes>}]`; `threshold_value` is duration in minutes.
  - **Must** also pass `schedule_type="fixed"` plus an `interval_minutes` (or `interval_crontab`) cadence; the threshold's meaning depends on how often the check runs.
1. **Always preview first**; show the user the planned changes per `(table, rule_type)` pair
- **NEVER** group multiple tables into one recommendation; one tool call per
- **NEVER** recommend tuning schema anomalies; they are not supported.
  args; AUTO preserves the platform schedule. Conversely, **always** pass
  config has any; omitting tags clears them.