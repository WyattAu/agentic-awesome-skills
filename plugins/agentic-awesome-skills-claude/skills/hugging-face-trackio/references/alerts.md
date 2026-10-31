- `TRACKIO_WEBHOOK_URL`; global webhook URL
- `TRACKIO_WEBHOOK_MIN_LEVEL`; minimum level for webhook delivery (`info`, `warn`, `error`)
            text=f"Loss {loss:.4f} still above 5.0 after {step} steps; learning rate may be too high",
            text="Nonfinite loss; preserve checkpoint and diagnose numeric/data errors",
Alerts are automatically printed to the terminal when fired. If the agent is watching the training script's output (e.g. running in the foreground or tailing logs), it will see alerts immediately; no polling needed.
# Alert fired at step 200: get all metrics in a ±5 step window