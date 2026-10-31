# complexity-cuts: Lower Big-O on Existing Code
If the code has no tests, you write a characterization test first (golden input → current output). Then transform. Then verify the test still passes. If you skip this, the optimization can silently break callers; and faster-but-wrong is worse than slow-and-right.
   4. Count reverts on this piece of code. If **3 reverts in a row**, STOP optimizing. The bottleneck is wrong, the transformation is wrong, or the code has invariants you have not modeled. Escalate to `invariant-guard` and write the missing contract; do not try a fourth transformation.
   If you cannot measure (e.g. the win is purely asymptotic on inputs you don't have), say so explicitly: `asymptotic only, no measurement; O(n²) → O(n)`. Never silently skip this step.
## Canonical example: workflow vs no-workflow
### Without the workflow: changes semantics AND patches the test
Faster, *and* changes the result set. Existing tests catch it; but the diff also "fixes" a flaky test by removing the assertion that checked the old behavior. Ships green. Breaks the billing report two weeks later.
### With the workflow: one transformation, semantics preserved
The example assumes finite integer IDs and side-effect-free ordinary objects; add duplicate-ID and missing-ID characterization cases before transforming. NaN IDs or getters require a different equivalence argument. One transformation. Existing tests stay untouched. Run them. If green, ship. If red, revert (don't patch). After 3 reverts, stop and load `invariant-guard`; the bottleneck is wrong, or the function has a contract no one wrote down.
When proposing or applying an optimization, your message must contain; in this order:
1. **Bottleneck**; file:line and one-sentence reason.
2. **Current complexity**; `time = O(?)`, `space = O(?)`.
3. **Transformation**; name from the playbook (or describe it if novel).
4. **New complexity**; `time = O(?)`, `space = O(?)`.
5. **Semantic risk**; anything callers might notice (ordering, stability, error timing). "None" is a valid answer if true.
6. **Measured speedup**; `before → after` with the ratio as `N× faster` (or `asymptotic only` if not measured). One line, honest numbers.
## Stop conditions: do not optimize further when
- The bottleneck is I/O or external service latency, not CPU/memory; go fix that instead.
| "I already solved this in my head; just paste the diff and add labels after." | Retrofitted labels lie about the reasoning order. Write bottleneck → complexity → transformation → diff in that order, or you are writing fiction. |
| "Stating the current Big-O is busywork; everyone can see the nested loop." | If everyone can see it, writing one line costs nothing. If only you can see it, you just saved the reviewer's time. |
| "Semantic risk is None, skip that step." | "None" is a valid answer; but write it. The next reader does not know which guarantees you considered. |
## Red flags: STOP
Cannot check every box? The optimization is not done. Either revert or finish the gap; do not ship a half-verified speedup.
- **Measurement is on the author.** complexity-cuts requires the ratio to be reported but does not run the benchmark for you; you must produce a representative input.
- **Won't help I/O-bound code.** If the dominant term is network latency or disk, the playbook will not move the needle; fix the I/O pattern instead.
> **Existing code earned its slowness one shortcut at a time. complexity-cuts removes them one transformation at a time; and refuses to ship the optimization without a green test.**
- `lemmaly`; prevention gateway; use when writing new code instead of refactoring existing.
- `invariant-guard`; escalation target when 3+ transformations have failed tests; the missing piece is a contract, not an optimization.
- `mathguard`; escalation when the classical floor is reached and an approximate or math-heavy structure could win.