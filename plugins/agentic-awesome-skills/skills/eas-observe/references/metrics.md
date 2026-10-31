# EAS Observe metrics: interpretation cheatsheet
> Source: https://docs.expo.dev/eas/observe/reference/metrics/; this is the canonical reference for metrics. Consult this page for the latest guidance, full prose definitions, optimization tips, and rationale.
All durations are in seconds. Metric data is retained for a minimum of 60 days. By default, every installation dispatches all of its events; high-volume apps can sample per installation with `configure({ sampleRate })`; see [Sampling](https://docs.expo.dev/eas/observe/configuration/#sampling).
| Cold launch | `expo.app_startup.cold_launch_time` | **< 1.5s** | Yes (native-only; JS code does not affect it) |
| Time to interactive (TTI) | `expo.app_startup.tti` | **< 3s** incl. cold launch | **No**; call `markInteractive()` once the screen is genuinely usable |
### Frame rate: what kind of slowness
| `expo.frameRate.totalDelay` | Total accumulated time (seconds) frames exceeded their target duration | Best single "smoothness" number; compare to TTI |
### Device state: is the regression environmental?
### Network state: is startup network-bound?
### Network requests: was the network the cause?
TTI events summarize the HTTP requests made during launch, from the end of the native launch to the `markInteractive()` call. Traffic is observed automatically; `URLSession` on iOS, `OkHttpClient` on Android, which covers `fetch`; and Observe's own uploads are excluded. All of these are omitted when the window held no requests.
- The `environment` tag (defaults to `process.env.NODE_ENV`) is metadata only; it does not gate dispatch by itself.