- **PASS**; fresh evidence satisfies the agreed criterion.
- **FAIL**; fresh evidence disproves the criterion.
- **BLOCKED**; the gate could not be tested because access, tooling, configuration, or an upstream dependency is unavailable.
- **NOT APPLICABLE**; the gate is deliberately excluded with a written reason.
SOURCE VALIDATION: PASS; revision 8f31c2a, tests 146/146
CI: PASS; required run 72814 completed at 2026-09-12T10:06:00Z
DEPLOYMENT IDENTITY: PASS; artifact maps to revision 8f31c2a
DATABASE MIGRATIONS: BLOCKED; target database access unavailable
LIVENESS: FAIL; HTTP 503 at 2026-09-12T10:14:22Z
READINESS: FAIL; HTTP 503 at 2026-09-12T10:14:24Z
CRITICAL ROUTES: BLOCKED; smoke checks stopped after readiness failure
OPERATIONAL SIGNALS: BLOCKED; log access unavailable
- `@verification-before-completion`; verifies that any completion claim has fresh supporting evidence; use this skill for the production-specific gate model and verdict.
- `@deployment-procedures`; use for executing a deployment; return here afterward to certify the deployed runtime.