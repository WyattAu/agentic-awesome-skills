Create the `docs/adr/` directory lazily; only when the first ADR is needed.
That's it. An ADR can be a single paragraph. The value is in recording *that* a decision was made and *why*; not in filling out sections.
- **Status** frontmatter (`proposed | accepted | deprecated | superseded by ADR-NNNN`); useful when decisions are revisited
- **Considered Options**; only when the rejected alternatives are worth remembering
- **Consequences**; only when non-obvious downstream effects need to be called out
1. **Hard to reverse**; the cost of changing your mind later is meaningful
2. **Surprising without context**; a future reader will look at the code and wonder "why on earth did they do it this way?"
3. **The result of a real trade-off**; there were genuine alternatives and you picked one for specific reasons
If a decision is easy to reverse, skip it; you'll just reverse it. If it's not surprising, nobody will wonder why. If there was no real alternative, there's nothing to record beyond "we did the obvious thing."
- **Technology choices that carry lock-in.** Database, message bus, auth provider, deployment target. Not every library; just the ones that would take a quarter to swap out.
- **Rejected alternatives when the rejection is non-obvious.** If you considered GraphQL and picked REST for subtle reasons, record it; otherwise someone will suggest GraphQL again in six months.