implementer's work; the only one you can commit from, and often the only copy at all. Never run
`git checkout`, `reset`, `clean`, or a branch switch in the workspace between those two points -
and open any untracked files (`??` in `git status`) directly; they are the implementer's new
the verdict can legitimately be to discard; work built on a premise you have since corrected,
for example; and then `git checkout`/`clean` is the right tool. The ban is on reflexive cleanup