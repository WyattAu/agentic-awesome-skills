Code you can't observe is code you can't operate. Observability is the ability to answer "what is the system doing and why?" from the outside, using the telemetry the code emits. Instrumentation is not a post-launch add-on; it's written alongside the feature, the same way tests are. If a feature ships without telemetry, the first user-reported bug becomes archaeology instead of a query.
- Diagnosing a failure happening right now; use the `debugging-and-error-recovery` skill (observability is what makes that skill fast next time)
- Profiling and optimizing measured slowness; use the `performance-optimization` skill
- Launch-day monitoring checklists and rollback triggers; see the `shipping-and-launch` skill; this skill covers the instrumentation that feeds them
If you can't name the questions, you're not ready to instrument; you'll log everything and learn nothing.
// BAD: string interpolation; unqueryable, inconsistent
**Log levels; use them consistently:**
**Never log secrets, tokens, passwords, or full PII.** This is a hard rule from the `security-and-hardening` skill; telemetry pipelines are a classic data-leak path. Allowlist fields; don't log whole request bodies.
As with tracing, the vendor-neutral path is the OpenTelemetry metrics API (same SDK and context as step 5). The example below uses Prometheus' `prom-client`; one common backend choice, not the only one; the RED/USE and cardinality rules are identical either way.
**Cardinality is the failure mode.** Every unique label combination is a separate time series. Labels must come from small, fixed sets (route template, status class, provider name). Never use user IDs, raw URLs, error messages, or other unbounded values as labels; that belongs in logs and traces.
Use OpenTelemetry; it's the vendor-neutral standard, and auto-instrumentation covers HTTP, gRPC, and common DB clients with near-zero code:
// tracing.ts; must be imported before anything else
Add manual spans only around meaningful internal units of work (e.g., `applyDiscounts`, `chargeProvider`) and attach the attributes on-call will filter by. Propagate context across every async boundary; HTTP headers, queue message metadata; or the trace dies at the gap. Choose a documented sampling policy. Tail sampling cannot recover spans dropped by an upstream head sampler; retaining every error requires the collector to receive those traces. Measure cost and data exposure.
2. **It links to a runbook**; even three lines: what it means, first query to run, escalation path.
- No correlation/request ID; each log line is an orphan