Apply F1-inspired standards to software testing. When brakes must work flawlessly at race pace, so must the code in production. This skill **executes** test plans and **files reports**; it is the driver, not the engineer. To plan and generate the suite, use **wjttc-builder**.
| **Brake** | 🚨 | Life-critical; failure is catastrophic | data loss, auth bypass, payment errors, destructive ops without confirm |
| **Engine** | ⚡ | Performance-critical; wrong results / poor UX | API accuracy, data transforms, calculations, format compliance, perf |
| **Aero** | 🏁 | Polish & edge cases; minor inconvenience | UI quirks, rare message formatting, optional-feature edges, docs |
| **Tyre** | 🛞 | Durability under load; degradation over time | stress/volume, concurrency, memory growth, large inputs |
| **Pit** | 🔧 | Release gate; the stop that lets you go | smoke/regression suite, CI green, the WJTTC report filed |
## Step 0: Signal Integrity pre-audit (run BEFORE adding/running anything new)
**Red CI is a contract: it must always mean "stop, look, fix."** A suite with high coverage but flaky reds is *less* trustworthy than a smaller suite with zero false alarms; because the team has stopped reading the reds. Fix the signal before you add more tests.
**Method**; classify the last 30 days of CI failures:
| **Infra** | Missing secret, runner image change, upstream dep; not the code | ✗ Workflow design defect |
| 100% | ✪ | Maintain; exemplary signal |
| 70–84% | ● Eroding | Stop adding tests; fix flakes first |
1. **Scope**; what should it do? happy path, edges, failure modes, perf targets, tier of each.
5. **Tier coverage check**; confirm every test is tiered:
6. **Report**; file the WJTTC report (below), then surface the tier verdict.
The FAF score is **deterministic**; same input, same score. A test report should be just as falsifiable: every verdict traces to a reproducible run. **FAF doesn't lie.**
- **Test with real data**, not just sanitized inputs; anonymized production data, messy inputs, production-like volume.
- **Tier before you test**; severity is the tier, so triage first; `faf wjttc` enforces that nothing ships untiered.
- [ ] Brake tests pass; zero tolerance
- [ ] `faf wjttc --strict` green; every test tiered
*Made with 🧡 by wolfejam.dev; "We break things so others never have to know they were broken."*