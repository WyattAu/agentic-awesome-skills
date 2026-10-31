- `<file>:<line>`; <tag>: <what's wrong> [`<quoted code or behavior>`]. Fix: <concrete change>.
<0–3 genuine, specific positives; omit on a clean review; do not manufacture praise>
One line per section: the findings it produced, or `clean` (walked it, found nothing). A blank section is an unbacked claim, not a pass; fill it before delivering.
- **Critical**; security, correctness, data loss, swallowed exceptions, hardcoded "success" returns.
- **Important**; design defects with maintenance cost: SOLID violations, premature abstractions, parameter explosion, generic naming.
- **Nit**; style, single-letter names outside loops, missing docstring contracts on public APIs.
Every finding carries its quoted code or observed behavior and a named fix; that is what lets the author contest it; with no quote or no fix it is not a finding, so drop it. Report only counted findings: never an estimated quality score, "X% cleaner," or a maintainability index; no baseline exists, so the number would be invented.
- **Refactor review:** the user wants the code to be cleaner, not different. **Observable behavior must not change**; same inputs, same outputs, same exceptions, same side effects. If you'd suggest a change that alters behavior, mark it as a *separate finding* labelled "Behavior change; confirm with author" and do not bundle it with refactor recommendations. Refactoring is *"a change made to the internal structure of software... without changing its observable behavior"* (Fowler, *Refactoring*).
### Section A: naming and functions
### Section B: comments and formatting
### Section C: SOLID
### Section D: DRY, KISS, YAGNI
### Section E: AI failure modes (highest leverage)
2. Any defensive guards for types/values the system already excludes; *inside* a trust boundary? (Validation of external or untrusted input at the boundary is not a defensive guard; do not flag it.)
3. Any premature abstraction; interface or factory with one implementation?
4. Any comment pollution; line-by-line restating, step-number scaffolding, or documentation comments that paraphrase signatures?
14. Any speculative configurability; flags, env vars, optional params without callers?
A finding must name its fix; a code change OR a specific structural action. "Nameable" is the bar, not "codeable." No named fix means it stays vague unease, not a finding; drop it.
- ❌ "This error handling could perhaps be more specific." (no named replacement; drop it)
If the user pushes back on a finding, cite the source from the relevant `references/` file. The rules trace to primary sources (Uncle Bob, Fowler, Hunt & Thomas, McCabe, Metz) and published 2024–2026 research on LLM code generation. If the user has a context-specific reason to override, record it as an inline comment that names **the principle, the reason, and a revisit trigger**; e.g. `// clean-code exception: 4-arg ceiling; config DTO, all fields required at construction; revisit when an optional field appears.` (the prefix is illustrative, not a required tag). On a later pass a well-formed marker downgrades the finding to *Documented exception*; don't re-flag it; a marker with **no revisit trigger is itself a finding**, since an exception with no exit is just deferred debt. Name the principle, not a rule number; a number is meaningless to a future reader.
- Execute the code or run tests. Add a finding instead: *"No tests for the new `charge` path; recommend adding."*