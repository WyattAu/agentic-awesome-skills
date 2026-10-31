Red CI is a contract: stop, look, fix. If red means *"shrug, runner had a noisy neighbor, just rerun,"* the signal is dead; and dead signal is worse than no signal at all. A test suite with 100% coverage but flaky reds is **less trustworthy** than one with 80% coverage and zero false alarms, because the team has stopped reading the reds.
| **Infra** | Red was missing secret, runner image change, dep upstream; not the code under test | ✗ Workflow design defect |
| 100% | TROPHY ✪ | Maintain; exemplary signal |
- **Hard absolute-time perf assertions on shared CI runners**; `expect(time).toBeLessThan(30)`. Move to non-gating workflow with `continue-on-error: true`.
- **Network-dependent tests in main suite**; mock at the boundary or route to integration tier.
- **Concurrency tests without explicit ordering**; use deterministic schedulers.
- **OS scheduler-dependent timing**; replace with statistical (P95 over N) or relative (vs same-run baseline) assertions.
- **Secret-dependent steps**; skip only explicitly optional checks; required deployment/security gates must fail closed or remain blocked when prerequisites are absent.
## Test Tier System: the WJTTC Five
WJTTC has **five** tiers: **Brake · Engine · Aero · Tyre · Pit**. The builder classifies every component into one of them. (`faf wjttc` audits a suite for the same five and flags untiered tests; name your tests with a tier word so the audit can place them.)
### Tier 1: BRAKE (Safety: Critical)
### Tier 4: TYRE (Live: the Real Road)
- Wear and durability; long-running sessions, repeated calls, soak/load behavior
- Degraded conditions; slow networks, partial data, rate limits, retries
### Tier 5: PIT (Operational: When Needed)
- Ops health; startup/shutdown, config loading, secrets present, observability
| 100% | Trophy | ✪ | Perfect; Gold Code |
## Integration: where the builder hands off