# Design direction, flows and screens: minimal by default
**User gave a reference** (Figma, screenshots, a product, a brand kit): extract and document; colours, type, spacing rhythm, component shapes, navigation model, tone of imagery. Keep what serves the personas; note every deviation you make and why (usually usability, RTL, contrast, or minimalism). The build sessions need the tokens, not a link to a Figma they cannot open.
- Standard components from a well-documented, actively maintained design system verified this session (RTL support and accessibility documented), themed lightly; no custom component library in v1.
- **Empty, loading, error, offline states designed for every list and screen**; they are screens, not afterthoughts.
### F-03: <Flow name> (P1)
**Job**; what the persona is trying to get done, in one line.
**Trigger**; how they arrive (deep link from SMS, home screen, notification).
**Preconditions**; logged in? has data? feature flag?
**Decision points**; at step n: if <condition> → branch A (steps …) else → branch B.
**Failure branches**; for each step: what can fail (from edge cases), what the user sees (copy IDs), how they recover, what is persisted.
**Exit**; where the flow ends and what state the system is in.
**Success metric**; M-x.y; time-to-complete target `[ASSUMED]` if hypothetical.
**Benchmark note**; which best-in-class behaviour this adopts (S-nn) and what we do differently.
Keep flows to the **minimum number of steps** that still respects the persona's decision points; count steps and taps and state them; that number becomes an acceptance criterion.
Screens are counted: a product with 40 screens in v1 has a scope problem; revisit section 2 before writing copy for all of them.
Colour: bg, surface, text-primary, text-secondary, accent, accent-text, success, warning, danger, border, focus-ring; as hex, with contrast ratios noted against their usual backgrounds.