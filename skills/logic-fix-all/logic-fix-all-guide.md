# Logic-Lens: Logic Fix All: Guide (Navigation)
  Phase 0; Pre-flight notice & consent gate        ← hard stop, wait for user
  Phase 1; Scope enumeration (role + risk tier)
  Phase 2; Health pass (module Logic Score map)
  Phase 3; Deep review (per-file semi-formal)
  Phase 4; Fault location (conditional)
  Phase 5; Path clarification (conditional)
  Phase 6; Fix queue assembly (sorted, remedied)
  Phase 7; Apply + verify (logic-diff)
  Phase 8; Iteration loop (until clean or capped)
  Phase 9; Final Fix Report
| **0, 1, 2** | `guide-phases-0-2-consent-scope-health.md`; Pre-flight consent; full-repo file walk by role and risk tier; module Logic Score map. |
| **3, 4, 5** | `guide-phases-3-5-review-locate-clarify.md`; Per-file Premises→Trace→Divergence (Phase 3); conditional fault localization (Phase 4); conditional path clarification (Phase 5). |
| **6, 7, 8, 9** | `guide-phases-6-9-fix-iterate-report.md`; Fix queue sorted by severity (Phase 6); apply + verify via logic-diff, revert on regression, retry ≤3× (Phase 7); iteration loop with state tracking (Phase 8); Final Fix Report (Phase 9). |
1. **Phase 0**; clarify only missing authorization/scope; reuse prior explicit consent.
2. **Phase 8 iteration cap**; when `fix_all.max_iterations` Warning/Suggestion rounds have run with remaining non-Critical findings. Three consecutive "continue"s hit a hard ceiling.
3. **Phase 6 constraint-vs-code tie**; when a finding could be fixed by editing either the code or a behavioral doc/config and neither side is clearly authoritative.