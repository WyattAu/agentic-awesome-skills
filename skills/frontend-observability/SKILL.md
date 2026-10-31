> Portable skill; readable by Claude Code, OpenCode, Codex, Cursor, Windsurf, and others.
> This skill describes a **field-side observability system**; event taxonomy, provider fan-out,
> real-user vitals, error reporting, consent; not a dashboard or a specific vendor. It is the
The goal: you can answer "what are real users doing, and what are they experiencing?"; with a
1. **Events are a typed vocabulary.** Event names are canonical constants with a union type; never inline string literals. The taxonomy is reviewable in one file and the compiler rejects typos.
2. **Fan-out is best-effort, not a scheduling guarantee.** `track()` dispatches to every provider, each with synchronous and asynchronous error handling. A missing global, a thrown provider, an unloaded script; ordinary caught exceptions/rejections do not propagate; synchronous vendor work can still block the thread.
4. **Field vitals complement lab budgets.** Real-user LCP/INP/CLS are reported to the same fan-out. Lighthouse samples controlled conditions; field vitals estimate actual experiences for the observed population; together they close the loop.
// CORRECT; typed constant, autocompletes, can't typo
// WRONG; stringly-typed, drifts, no compile check
- Event names are snake_case and stable; renaming one breaks historical dashboards, so treat the catalog as a contract.
  if (!hasConsent()) return; // §6; nothing fires before opt-in
Each adapter is a tiny `(event, props) => void` that **guards its provider global**; it no-ops on
### 3.1 Firebase Analytics: one adapter, two platforms
single conceptual adapter covers both web and React Native; only the import and the "is it
// services/analytics/adapters.firebase.web.ts; Firebase JS SDK (web)
// services/analytics/firebase.init.ts; lazy, browser-only init (web)
// services/analytics/adapters.firebase.native.ts; @react-native-firebase/analytics (RN / Expo)
across platforms. Verify Firebase event-name/parameter restrictions and reserved names for the pinned SDK. Snake_case is this taxonomy convention, not the full provider validation contract. Gate `initFirebaseAnalytics()` on consent (§6); Firebase also exposes
fails to load, a global that isn't there yet, an adapter that throws on a malformed prop; all are
// a tracked leaf; Server Components can't use the hook, so wrap in a thin client component
The provider does **no work during render**; `track` is stable and adapters guard their own
`window` access; but still verify the framework-specific client boundary and actual initialization effects.
- Lab budget green + field "poor" = a gap between your test conditions and real devices/networks; exactly what field RUM exists to reveal.
Telemetry fires only after opt-in, checked **once at the fan-out boundary** (§3); not duplicated at
- `track()` early-returns when consent is absent; no events, no vitals, no error PII before opt-in.
adapter), at **deliberate boundaries**; not a global swallow.
| **Expo / React Native**  | swap the web-vitals source for RN performance APIs and the web provider scripts for native SDKs (**`@react-native-firebase/analytics`**, Amplitude, PostHog-RN); the **taxonomy, `track` fan-out, consent gate, and `useAnalytics` hook are unchanged**. The Firebase adapter is the same shape; it guards the native module instead of `window` (see §3.1). |
- [ ] Event names are canonical constants with a union type; zero inline event strings.
- [ ] Each adapter call is individually try/caught; one provider can't break the app or the others.
- [ ] Mutating the adapter registry (tests) is the dispatch-observation seam; no real provider mounted in tests.
taxonomy and fan-out are untouched; Firebase is just one more entry in `analyticsAdapters`.
2. Keep the frontmatter `name` and high-signal `description`; discovery indexes match against it.