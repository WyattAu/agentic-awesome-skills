1. **Institutional memory**; why a feature was rejected, so the reasoning isn't lost when the issue is closed
2. **Deduplication**; when a new issue comes in that matches a prior rejection, the skill can surface the previous decision instead of re-litigating it
The file should be written in a relaxed, readable style; more like a short design document than a database entry. Use paragraphs, code samples, and examples to make the reasoning clear and useful to someone encountering it for the first time.
- #42; "Add dark mode support"
- #87; "Night theme for accessibility"
- #134; "Dark theme option"
The reason should be substantive; not "we don't want this" but why. Good reasons reference:
The reason should be durable. Avoid referencing temporary circumstances ("we're too busy right now"); those aren't real rejections, they're deferrals.
- Matching is by concept similarity, not keyword; "night theme" matches `dark-mode.md`
- If there's a match, surface it to the maintainer: "This is similar to `.out-of-scope/dark-mode.md`; we rejected this before because [reason]. Do you still feel the same way?"
- **Confirm**; the new issue gets added to the existing file's "Prior requests" list, then closed
- **Reconsider**; the out-of-scope file gets deleted or updated, and the issue proceeds through normal triage
- **Disagree**; the issues are related but distinct, proceed with normal triage
Only when an **enhancement** (not a bug) is *rejected* as `wontfix`. This applies to enhancement PRs exactly as it does to issues; a rejected PR is recorded here so the same request doesn't return as fresh code.
- The skill does not need to reopen old issues; they're historical records