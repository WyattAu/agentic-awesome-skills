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
## Step 4: Build the app
> By default, metrics collected from **debug builds** are not dispatched. A build is treated as a debug build when either the native app is a debug build or the JS bundle is a development bundle (`__DEV__` is `true`). To dispatch anyway while testing the integration, set `dispatchInDebug: true` when calling `configure()`; see [Enable metrics in development](https://docs.expo.dev/eas/observe/configuration/#enable-metrics-in-development). This has no effect on release builds.
## Step 5: View the metrics
## Optional: per-route navigation metrics (SDK 56+)
1. Enable the integration at module scope, **before any screen mounts** (it cannot be toggled at runtime; calling `configure()` after mount throws):
2. Replace the top-level `<NavigationContainer>` with `<ObserveNavigationContainer>`; a drop-in replacement that accepts the same props and forwards the same ref. If you pass a `linking` config it is used to resolve a human-readable screen path; otherwise the metric falls back to `route.name`.
In both integrations, `useObserve()` is safe to leave in place even when the integration is disabled or the router package is absent; it falls back to the global `markInteractive`.
- [ ] `markInteractive()` called from every entry screen once it is genuinely interactive; global `AppMetrics.markInteractive()` on SDK 55, or `useObserve()` hook on SDK 56+.