- **One report per invocation.** Always end by writing the `.md` file (even if the verdict is "Not affected"; the audit trail matters).
2. **Run checks in parallel** (Bash tool, multiple calls in one message). Pick relevant checks for the advisory type; don't run all of them.
# <Subject>: Audit
- **<ID or source> "<Name>"**; <one-line description>. <Affected versions or scope>.
- **Not observed in checked scope.**; absent/patched within the explicitly searched paths; state coverage gaps. A stopped process alone does not prove absence of vulnerability.
- **Affected.**; vulnerable version present *and* reachable by the attack vector.
- **Mitigated exposure.**; affected version present with an observed mitigation; stopped services or loopback listeners may still be reachable by another attack path. State conditions and gaps.
- **Inconclusive.**; advisory/version/coverage evidence missing. Do not infer safety from empty output or suppressed errors.
- `baseline-audit.md` (long-form baseline audit; different format, do not mimic)