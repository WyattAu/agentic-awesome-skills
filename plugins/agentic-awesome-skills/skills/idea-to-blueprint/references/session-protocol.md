# Session protocol: one epic per fresh session
Long agent sessions accumulate drift: forgotten constraints, half-remembered decisions, context compression. Starting each epic in a clean session forces every needed fact to live in the repo, where it is versioned, greppable and reviewable. The cost is that nothing can be assumed to be remembered; so the protocol makes the repo the memory: the blueprint says *what*, PROGRESS.md says *where we are*, DECISIONS.md says *why*, and `check` says *whether it works*.
`AGENTS.md` and `CLAUDE.md` are identical in content and short (≤ 60 lines): where the docs are, the protocol in ten lines, the `check` command, the commit format, and the rule "if it's not in BLUEPRINT or PROGRESS, ask or record; don't assume".
**Boot (user)**; open a fresh session in the repo, paste the session prompt for the epic (Appendix A of the blueprint; generated from `assets/session-prompt.md`).
**Build (agent)**; for each story in order:
9. Implement the tasks; run `check` after each task or story; keep it green. Copy strings come from the copy table by ID; no improvised strings.
12b. **Self-review.** Walk the review checklist (BLUEPRINT §14.4) over the epic's diff, story by story, and answer every item. Do this as if reviewing someone else's work: read the diff, not your memory of writing it. Fix what fails rather than noting it. Report the result in the handoff summary as `reviewed: <n> items, <n> fixed, <n> accepted with reason`; never as a bare ✔. This pass exists because the human reviewer sees a large diff produced quickly, and their attention is the scarcest resource in the whole process; anything you can catch here is worth ten times what it costs.
**Terminate (user)**; close the session. Open a new one for the next epic. Repeat.
- The review checklist (§14.4) applied to every story, with the result recorded; not "reviewed", but which items failed and what was done.
## Handoff: E03 <Title>: <date>
  1 accepted: duplication detector flags the two DTO mappers; kept, extraction is E05's job.
Diff size: E03-S02 was 620 lines, over the 400 limit; story was mis-sized, amendment A-03 splits it.
Deviations: E03-S02 AC-4 rate limit changed 5→10 per 10 min; see Amendment A-02, DR-07
- Evidence lines are specific: `AC-3 ✔ api/auth_otp_request.test::provider_timeout_returns_503` or `AC-6 ✔ manual 2026-09-05; Persian digits render on Chrome Android 128`.
The blueprint is the source of truth, so when reality disagrees with it, the blueprint changes; explicitly:
5. Scope additions are not amendments; they are new stories in a later epic; record the idea in PROGRESS.md "Parking lot" and move on.
- **`check` red at boot**: the previous session violated a gate. Fix the red first as a hotfix story `E03-S00; restore green` with its own evidence, then continue.
- Skills: Claude Code discovers `SKILL.md` folders under `.claude/skills/` (project) and `~/.claude/skills/` (personal); Codex discovers the same open skill format from its skills locations (`~/.codex/skills/`, `.codex/skills/`, and `.agents/skills/` in recent versions; confirm against the current Codex docs when installing). The session protocol does not depend on skills at build time; it depends only on the repo files.