wgm ("well, gosh... make") is a portable build **methodology**, not a domain skill; a single
  have complete, unambiguous step-by-step instructions; wgm explicitly stays out of the way there.
Classify the work onto a scale-adaptive track (Quick / Standard / Full) so ceremony matches risk -
success criteria, and constraints are known; capping interrogation after ~5 questions to avoid
path), holdout acceptance scenarios the build must never read, and `IMPLEMENTATION_PLAN.md`; the
dimension; do not start building on a shaky plan.
`IMPLEMENTATION_PLAN.md`, scores Preflight readiness, then loops one task at a time; each task's
own test/lint/build command must exit 0 before it's marked done; and finally ships with a
- useful when a human wants to review the plan before any code is touched.
- Do let the plan be the shared state; a fresh agent should be able to resume a build from
  move faster; misalignment discovered after building is far more expensive.
- Don't treat a high satisfaction score as sufficient on its own; a failing deterministic check
- wgm is a protocol, not a runtime: it has no daemon, scheduler, or bundled dashboard; it expects
  does not need; wgm's own Triage track exists specifically to right-size this, and the skill
  **Solution:** Scenarios are read only during Validate/Review, never during Implement; that's the