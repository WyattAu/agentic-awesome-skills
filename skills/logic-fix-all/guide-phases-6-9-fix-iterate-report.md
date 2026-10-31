# Logic-Lens: Logic Fix All: Phases 6-9 (Fix · Verify · Iterate · Report)
## Phase 6: Fix Queue Assembly
6a. Merge all findings from Phases 3–5 (Phase 3 review + Phase 4 locate, as updated/filtered by Phase 5 clarification). Phase 2 health observations are not directly included; they must first receive a full Premises→Trace→Divergence triple from Phase 3 before entering the queue (Iron Law).
- **Code vs runtime config**: edit the CONFIG. Exception: if the config value is internally coherent for its key AND the code looks typo'd, edit the CODE. When both sides are plausible, record as "Unresolved; unclear whether spec or consumer is wrong".
- **Doc vs doc**: establish explicit authority and intended behavior; timestamp/path depth alone do not settle specification conflicts; if still tied → "Unresolved; ambiguous spec" with both citations.
## Phase 7: Apply + Verify (logic-diff)
| Semantically Equivalent |; | fix changed nothing | 7d |
| Semantically Divergent |; | fix broke previously-correct paths | 7d |
Never use `git reset --hard` or `git clean -f`. After 3 failed attempts, record as "Unresolved; conflicting constraints" and continue.
7e. If logic-diff cannot confirm equivalence (function too complex or involves external state), note as "unverified; integration test recommended" and continue.
## Phase 8: Iteration Loop
- **`unresolvable_findings`** (set): findings Phase 7d retired with "Unresolved; conflicting constraints". Each entry is `(file_path, line_range, L_code, divergence_signature)`. Match primarily on `(file_path, line_range, L_code)`; `divergence_signature` is a tie-breaker only (LLM-generated Divergence text can drift in wording).
(You have continued {consecutive_continues} time(s) so far; hard
- **Consent:** increment `consecutive_continues`. If now ≥ 3, hard stop; record remaining as "Unresolved; hard iteration ceiling reached (user continued 3×)" and go to Phase 9. Otherwise reset `non_critical_round_counter` to 0 and return to 8b.
- **Negation (or non-consent):** record remaining as "Unresolved; user stopped iteration at round N" and go to Phase 9.
## Phase 9: Final Report
Do not output per-finding Premises/Trace/Divergence blocks in the final report; the Fix Log table is the user-facing record. Provide full trace on request.