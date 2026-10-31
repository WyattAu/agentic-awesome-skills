Insert `trackio.alert()` calls in training code to flag important events; like inserting print statements for debugging, but structured and queryable:
- `trackio.alert(title="...", level=trackio.AlertLevel.WARN)`; fire an alert
- `trackio list projects/runs/metrics`; discover what's available
- `trackio get project/run/metric`; retrieve summaries and values
- `trackio list alerts --project <name> --json`; retrieve alerts
- `trackio show`; launch the dashboard
- `trackio sync`; sync to HF Space
1. **Set up training with alerts**; insert `trackio.alert()` calls for diagnostic conditions
2. **Launch training**; run the script in the background
3. **Poll for alerts**; use `trackio list alerts --project <name> --json --since <timestamp>` to check for new alerts
4. **Read metrics**; use `trackio get metric ...` to inspect specific values
5. **Iterate within an authorized run/time/cost budget**; preserve checkpoints, inspect alerts, and decide whether a controlled change and new run are justified
            text="Loss near zero; inspect objective, data leakage and gradient metrics before diagnosing",