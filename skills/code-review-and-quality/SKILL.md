Multi-dimensional code review with quality gates. Every change gets reviewed before merge; no exceptions. Review covers five axes: correctness, readability, architecture, security, and performance.
**The approval standard:** Approve a change when it definitely improves overall code health, even if it isn't perfect. Perfect code doesn't exist; the goal is continuous improvement. Don't block a change because it isn't exactly how you would have written it. If it improves the codebase and follows the project's conventions, approve it.
- **Is a new conditional bolted onto an unrelated flow?** That's a design smell, not a nit; push the logic into its own helper, state, or policy instead of tangling an existing path.
- **Does this refactor reduce complexity or just relocate it?** Count the concepts a reader must hold to follow the change. If a "cleaner" version leaves that count unchanged, it isn't cleaner; prefer the restructuring that makes whole branches, modes, or layers disappear over one that re-centralizes the same logic. Prefer deleting an abstraction to polishing it.
- **Are type boundaries explicit?** Question gratuitous `any`, unchecked casts or unjustified optionality; `unknown` is appropriate at untrusted boundaries when narrowed before use. Inspect and silent fallbacks that paper over an unclear invariant; making the boundary explicit often makes the surrounding control flow simpler.
When you flag a structural problem, propose the move; not just the problem. A review that only says "this is complex" leaves the author guessing. Reach for a named restructuring:
**Watch file size, not just diff size.** A small diff can still push a file past a healthy boundary; around 1000 *total* lines in a single file (distinct from the ~1000 *changed*-lines threshold above) is a common inspection signal, not a hard cap. When a change materially grows an already-large file, ask whether to extract helpers, subcomponents, or modules *first*, before piling more on. Decompose, then add.
**What counts as "one change":** A single self-contained modification that addresses one thing, includes related tests, and keeps the system functional after submission. One part of a feature; not the whole feature.
**Separate refactoring from feature work.** A change that refactors existing code and adds new behavior is two changes; submit them separately. Small cleanups (variable renaming) can be included at reviewer discretion.
| **Nit:** | Minor, optional | Author may ignore; formatting, style preferences |
| **FYI** | Informational only | No action needed; context for future reference |
**Lead with what matters.** Order findings by leverage: correctness and security first, then structural regressions and missed simplifications, then everything else. Don't bury a real issue under cosmetic nits; a few high-conviction comments beat a long list. If you have one structural problem and ten nits, the structural problem *is* the review.
This catches issues that a single model might miss; different models have different blind spots.
Don't leave dead code lying around; it confuses future readers and agents. But don't silently delete things you're not sure about. When in doubt, ask.
- formatLegacyDate() in src/utils/date.ts; replaced by formatDate()
- OldTaskCard component in src/components/; replaced by TaskCard
- LEGACY_API_URL constant in src/config.ts; no remaining references
- **Respond within one business day**; this is the maximum, not the target
When reviewing code; whether written by you, another agent, or a human:
- **Accept override gracefully.** Reconsider with the author’s evidence; unresolved security/correctness concerns remain visible for the authorized maintainer under repository policy, rather than silently approving on assertion. Comment on code, not people; reframe personal critiques to focus on the code itself.
- [ ] **Approve**; Review recommendation only; exact-head required checks, policy and authorized merge still apply
- [ ] **Request changes**; Issues must be addressed
| "We'll clean it up later" | Later never comes. The review is the quality gate; use it. Require cleanup before merge, not after. |
| "The refactor makes it cleaner" | Relocating complexity isn't reducing it. If the reader still holds the same number of concepts, the structure didn't improve; look for the version where branches disappear. |
- Review comments without severity labels; makes it unclear what's required vs optional
- Accepting "I'll fix it later"; it never happens