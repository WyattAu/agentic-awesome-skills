# Logic-Lens: Logic Fix All: Phases 0-2 (Consent · Scope · Health)
## Phase 0: Pre-flight Notice & Consent Gate
0a. Reuse existing explicit scope/budget authorization; do not repeat a consent prompt already answered. If authorization is missing, estimate file count (`git ls-files | wc -l` in a git repo; `find . -type f -not -path '*/.git/*' -not -path '*/node_modules/*' -not -path '*/target/*' -not -path '*/.venv/*' -not -path '*/build/*' -not -path '*/dist/*' -not -path '*/vendor/*' | wc -l` otherwise). Display this notice verbatim with the estimate filled in; do not paraphrase:
⚠️  /logic-fix-all; Full Repository Logic Audit & Fix
Method:   Semi-formal execution tracing; Premises → Trace →
1. Hard negation → abort: "Aborted by user before scan; no files modified".
## Phase 1: Scope Enumeration
- `go.mod` → exclude `vendor/` (unless project-owned; check `modules.txt`)
- Respect `.gitignore` as a hint, not absolute; some ignored paths may still be relevant.
- **Runtime config:** `.json`, `.yaml/.yml`, `.toml`, `.ini`, `.conf`, `*.config.js/ts`; verify by grepping the codebase for the filename before classifying.
## Phase 2: Health Pass (logic-health)
2b. Record Phase 2 output for reference. Do NOT write remedies yet; health gives shape, not precision. Precise findings come from Phase 3.
2c. If the health pass reveals a systemic pattern (same L-code in 4+ modules), earmark the representative file for Phase 3 priority review. Phase 3 must produce the full Premises→Trace→Divergence triple before the pattern can enter Phase 6 as a fix candidate; a systemic observation without a trace cannot justify a remedy.