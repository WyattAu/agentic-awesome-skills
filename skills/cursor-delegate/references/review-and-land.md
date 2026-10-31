- **Scope creep**; changes the brief excluded.
- **Scope shortfall**; missed behavior, edges, or cleanup.
- **Quiet judgment calls**; defensible but unasked decisions that need review.
implementer's work; the only one you can commit from, and often the only copy at all. Never run `git checkout`, `reset`, `clean`, or a branch switch in the
workspace between those two points; however messy an interrupted run looks, inspect it first:
`git diff` is blind to the index), and open any untracked files (`??` in `git status`) directly -
verdict can legitimately be to discard; work built on a premise you have since corrected, for
example; and then `git checkout`/`clean` is the right tool. The ban is on reflexive cleanup
A resumed run carries the same autonomy flags as a fresh one; write-capable with `--force` by