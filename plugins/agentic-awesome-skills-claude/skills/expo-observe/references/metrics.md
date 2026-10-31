# EAS Observe metrics: interpretation cheatsheet
> Source: https://docs.expo.dev/eas/observe/reference/metrics/; this is the canonical reference for metrics. Consult this page for the latest guidance, full prose definitions, optimization tips, and rationale.
| Cold launch | `expo.app_startup.cold_launch_time` | **< 1.5s** | Yes (native-only; JS code does not affect it) |
| Time to interactive (TTI) | `expo.app_startup.tti` | **< 3s** incl. cold launch | **No**; call `markInteractive()` once the screen is genuinely usable |
| `expo.frameRate.totalDelay` | Total accumulated time (seconds) frames exceeded their target duration | Best single "smoothness" number; compare to TTI |
- The `environment` tag (defaults to `process.env.NODE_ENV`) is metadata only; it does not gate dispatch by itself.