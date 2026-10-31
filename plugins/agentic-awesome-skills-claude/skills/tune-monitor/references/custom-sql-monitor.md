- Alert conditions; each has an `operator` and either a `threshold_value` (explicit) or ML
- Anomalies are consistently marginal; the observed value just barely crosses the threshold
2. **Always dry-run first** (`dry_run=True`, the default); show the user the YAML preview
   a deep link in `result.instructions`; surface that to the user. `result.yaml` is `None` on
- **NEVER** omit `monitor_uuid`; this creates a duplicate monitor instead of updating.
  the monitor's configuration; fields you omit revert to tool defaults, they are NOT left
  rendered YAML returned in `result.yaml` against the original config; if anything you meant to