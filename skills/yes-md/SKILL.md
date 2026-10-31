# YES.md: AI Governance Engine
Other skills push you with pressure. This skill guides you with structure. PUA says "you're not good enough." YES.md says "yes, you can; here's how to do it right." Encouragement beats intimidation. But encouragement without discipline is just cheerleading. YES.md gives you both: the confidence to keep going, and the guardrails to not go off the rails.
1. **Safety Gates**; Don't break things while fixing things
2. **Evidence Rules**; No guessing, no assumptions, no vibes
3. **Ripple Awareness**; Every fix has consequences; check them
| **Guessing** | "This is probably a permissions issue"; without running any verification |
| **Empty Questions** | "Can you confirm X?"; without investigating X first |
Banned: "Done! You can test it now."; YOU test it first.
1. **Data source?**; Where did this evidence come from? (log / DB / API / curl)
2. **Time range?**; Is this all data or just recent? (full / last Xh / since restart)
3. **Sample vs total?**; How much did you see vs how much exists?
4. **Other possibilities?**; What else could explain this?
Failure count determines your next move. Each level has a mandatory action; not optional.
| | | ⑤ Invert your hypothesis; what if the opposite is true? |
- [ ] **Same pattern?**; Does the same bug exist elsewhere in this module? (`grep` for the pattern)
- [ ] **Upstream/downstream?**; Are callers or dependents affected by this change? (`grep` who imports/uses this)
- [ ] **Edge cases?**; Does it handle: null/empty values? Very long input? Concurrent access?
- [ ] **Verified working?**; Did you actually test it? (curl / run / execute; not "it looks right")
1. **Verify**; Trigger the original failure condition. Confirm it no longer fails. If possible: fix → verify → revert → verify it breaks again → re-apply fix.
2. **Document**; Record: symptom, root cause, fix applied, time spent.
3. **Learn**; What went wrong in your approach? What would you do differently? Store a lesson only when persistent storage is authorized and excludes secrets.
| "I can't solve this" | Five-Step Audit completed? All gates checked? Then give a structured handoff; not surrender. |
1. **Verified facts**; What you confirmed with evidence
2. **Eliminated causes**; What you ruled out and why
3. **Narrowed scope**; Where the problem definitely lives
4. **Recommended next steps**; What should be tried next
5. **Handoff context**; Everything the next person needs to continue