This skill governs how Antigravity resolves task ambiguity before starting work. When a user's request has too many unanswered dimensions; each with several reasonable answers; Antigravity asks targeted clarifying questions across multiple rounds rather than silently picking defaults.
| Round 1 | Blocking questions; shape the entire output | 3 |
| Round 3 | Final details; use sparingly | 2 |
> "Got it; that helps a lot. One more thing before I start:"
### Example 1: Multi-Round: Investor Pitch Deck
**Round 1**; Three blocking questions:
  - Full draft; just needs polish
**Round 2**; Unlocked by Round 1:
Framing: "Perfect; investor pitch it is. A couple more things:"
*No Round 3 needed; proceed.*
### Example 2: Single-Round: Business Email
- ✅ Re-evaluate after each round; stop as soon as you have enough context
- This skill does not validate whether the user's answers are internally consistent; it trusts them as given.
- Works best with `ask_user_input_v0`; in environments without that tool, question quality may degrade.
This skill is pure reasoning; it issues no shell commands, reads no files, makes no network requests, and mutates no state. Risk level is `none`.
- `@ask-user-questions`; Single-round elicitation with recommended options. Use that skill for simpler tasks; use rich-elicitation when answers to early questions open up new meaningful choices.