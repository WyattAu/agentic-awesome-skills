> Source: https://docs.expo.dev/eas/observe/get-started/; consult this page for the latest guidance.
## Step 1: Install the library
## Step 2: Wrap the root layout
**SDK 55**; use `AppMetricsRoot`:
**SDK 56 and later**; use `ObserveRoot`:
**Without Expo Router** (`App.tsx`): wrap the default-exported `App` component the same way; `export default AppMetricsRoot.wrap(App);` on SDK 55, or `export default ObserveRoot.wrap(App);` on SDK 56+.
## Step 3: Mark the app as interactive
TTI is **not** collected automatically. Signal it once the screen is genuinely ready for the user; i.e. after splash-screen-blocking work like update checks, authentication, initial data fetching, or splash animations finishes. Place the call in a `useEffect` that runs once that work resolves.
**SDK 55**; call the global `AppMetrics.markInteractive()`:
**SDK 56 and later**; use the `useObserve()` hook to get a bound `markInteractive`:
`markInteractive()` is safe to call repeatedly; only the **first** call per session is recorded. If the app has more than one entry screen (onboarding, login, deep-link targets), call `markInteractive()` on **each one**. Otherwise TTI will be missing for sessions that open via a deep link to a screen without the call.
`markInteractive()` accepts an options object. Use `params` to slice TTI by an app-specific dimension (cohort, tenant, feature-flag variant, cache hit). Use `routeName` to override the route the event is tagged with; useful when the logical screen name differs from the router path, when the route is dynamic, or when the app does not use Expo Router. Param values may be strings, numbers, booleans, or any JSON-serializable value.
## Step 4: Build the app
> By default, metrics collected from **debug builds** are not dispatched. A build is treated as a debug build when either the native app is a debug build or the JS bundle is a development bundle (`__DEV__` is `true`). To dispatch anyway while testing the integration, set `dispatchInDebug: true` when calling `configure()`; see [Enable metrics in development](https://docs.expo.dev/eas/observe/configuration/#enable-metrics-in-development). This has no effect on release builds.
## Step 5: View the metrics
## Optional: per-route navigation metrics (SDK 56+)
1. Enable the integration at module scope, **before any screen mounts** (it cannot be toggled at runtime; calling `configure()` after mount throws):
   **Dynamic configuration**; replace the top-level `<NavigationContainer>` with `<ObserveNavigationContainer>`. It is a drop-in replacement that accepts the same props and forwards the same ref. If you pass a `linking` config it is used to resolve a human-readable screen path; otherwise the metric falls back to `route.name`.
   **Static configuration**; `createStaticNavigation()` renders the container for you, so there is nothing to replace. Create the navigation ref yourself, pass it to both `<Navigation>` and `<ObserveNavigationProvider>`, and the provider records the same timings by listening through that ref.
In both integrations, `useObserve()` is safe to leave in place even when the integration is disabled or the router package is absent; it falls back to the global `markInteractive`.
## Optional: user-defined events (SDK 56+)
Beyond the automatic startup and navigation metrics, you can record your own named events from anywhere in the app to track product moments; a completed onboarding, an exported report, a selected item. Use `Observe.logEvent(name, options?)`.
> Source: https://docs.expo.dev/eas/observe/events/; consult this page for the latest guidance.
`logEvent` is a plain function call; it is **not** a hook and needs no `useObserve()`. Call it from event handlers, effects, or any non-render code. The event is persisted on-device and dispatched on the next flush (see dispatch notes below); the call returns immediately and never blocks the UI.
**Attributes** are the primary way to make an event queryable; attach the identifiers and measurements you'll want to filter or break down by:
- **Use lowercase, dot-separated names** (`task.completed`, `onboarding.skipped`). Keep them stable; `report_exported` and `report.exported` bucket as two separate events in the dashboard.
User-defined events are persisted on-device, batched, and dispatched on the next flush as **OpenTelemetry log records**; the same delivery path and timing as other metrics. The debug-build caveat from [Step 4](#step-4--build-the-app) applies unchanged: debug builds don't dispatch unless `configure({ dispatchInDebug: true })` is set.
User-defined events appear under the **Events** tab in the Observe dashboard, and are queryable from the terminal with `eas observe:events`; see [`./queries.md`](./queries.md).
## Optional: error reporting
`expo-observe` records errors as non-fatal `exception` log events alongside the performance metrics. They arrive by three paths, and they are **not** a replacement for a crash reporter such as Sentry; EAS Observe has no crash reporting yet.
**1. Unhandled JS errors; automatic.** A global `ErrorUtils` handler is installed when the package is first imported, which is earlier than any `configure()` call, so errors thrown before configuration are still recorded. React Native's own behavior is unchanged: the red box in development and fatal termination in production still happen. Turn the recording off with `configure({ errorHandlingEnabled: false })`.
**2. Render-phase errors; `<ObserveErrorBoundary>`.** Render errors never reach `ErrorUtils`, so a boundary is the only way to capture them with the React component stack. The boundary records the error and then renders `fallback` in place of the subtree.
`fallback` is required and accepts a React element, `null`, or a function receiving `{ error, resetError }`. `resetError()` clears the caught error and re-mounts the children, so they restart from a clean state. There is no capture-and-rethrow mode; a boundary always renders one of the three fallback forms. Errors that no boundary catches still reach the global handler.
**3. Handled errors; `Observe.reportError(error)`.** Report failures your code caught and recovered from, which reach neither the global handler nor a boundary.
The value is normalized before it is sent: an `Error` contributes `name`, `message`, and `stack`; any other thrown value (a string, a plain object, a number) is stringified into the message with no stack. `reportError` never throws; it is called from a `catch` block, so a failure inside it must not turn a handled error into an unhandled one.
## Optional: runtime configuration
**Network request monitoring is automatic and has no off switch.** `expo-observe` observes `URLSession` traffic on iOS and `OkHttpClient` traffic on Android from launch, and attaches a rollup of the launch window to the TTI event; including the **host** of the slowest request (see [`./metrics.md`](./metrics.md)). Observe's own uploads are excluded. No `configure()` option disables it as of `expo-observe` 57.0.9, so treat request hosts as data that leaves the device.
**Manual flush.** Events dispatch automatically when the app backgrounds; on Android through a background worker once connectivity returns, on iOS when the app resigns active or is about to terminate. Call `await Observe.dispatchEvents()` to flush early, which is mainly useful while testing.
- [ ] `markInteractive()` called from every entry screen once it is genuinely interactive; global `AppMetrics.markInteractive()` on SDK 55, or `useObserve()` hook on SDK 56+.