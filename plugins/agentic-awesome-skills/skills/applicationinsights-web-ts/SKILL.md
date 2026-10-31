Use this skill when you need instrument browser/web apps with the Application Insights JavaScript SDK (@microsoft/applicationinsights-web). Use for Real User Monitoring (RUM); page views, clicks, AJAX/fetch dependencies, exceptions, custom events, and browser-side GenAI agent traces with explicitly verified backend correlation.
Typings ship with the package; no separate `@types/...` install needed.
The browser SDK requires a connection string at init time. **It ships in plaintext to clients**; Microsoft Entra ID auth is not supported for browser telemetry. Use a separate App Insights resource with local auth enabled for browser RUM if you need to isolate it from backend telemetry.
# Vite / CRA / Next.js: expose to client via the public env prefix
Call `loadAppInsights()` exactly once, as early as possible (before user interactions you want tracked). Then `trackPageView()` for the initial load; when `enableAutoRouteTracking` is on, subsequent route changes are automatic.
// Dependencies (manually-tracked outbound calls; fetch/XHR are auto-tracked)
// User identity (set ONCE per authenticated session; values are PII; do not pass emails)
## Performance: Web Vitals
3. **Connection string is public**; never reuse the same App Insights resource for backend secrets.
6. **GenAI sensitive content** (`gen_ai.input.messages` etc.) is Opt-In; never log without an explicit runtime flag and approved data handling.
7. **Agent token usage is on `chat` spans, not `invoke_agent`**; copy aggregated usage to the parent agent span only if you know it.
8. **React StrictMode** double-invokes effects in dev; guard `loadAppInsights()` with a module-level singleton.
5. **For GenAI**, follow OTel `gen_ai.*` attribute names verbatim; they are queryable across browser and backend telemetry uniformly.
- [references/agent-traces.md](https://github.com/microsoft/skills/tree/main/.github/plugins/azure-sdk-typescript/skills/applicationinsights-web-ts/references/agent-traces.md); Full OTel GenAI semconv distilled (agent / model / tool spans, attributes, content capture).
- [references/framework-extensions.md](https://github.com/microsoft/skills/tree/main/.github/plugins/azure-sdk-typescript/skills/applicationinsights-web-ts/references/framework-extensions.md); React, React Native, Angular, Next.js, Vite recipes.
- [references/configuration.md](https://github.com/microsoft/skills/tree/main/.github/plugins/azure-sdk-typescript/skills/applicationinsights-web-ts/references/configuration.md); Full `IConfiguration` reference and tuning guide.