Use whenever building or styling web UIs; components, pages, dashboards, landing pages, React/Vue/HTML-CSS layouts; or whenever the user asks to make something "look better/nicer", fix spacing/layout, or mentions styling, color, typography, fonts, responsive design, polish, or aesthetics, even without the word "design".
**Before reporting any design "done": render it and have a *separate* judge critique the image** (not the code, not self-grading); see design-spatial §1. Blind generation can't see its own collisions; this applies to all design output, not just spatial work.
> **MANDATORY HORIZONTAL-OVERFLOW GATE; runs before ANY web UI is "done".**
> If you haven't measured narrow, you are not done; don't claim it.
- **design-thinking**; Load for every design task. Defines purpose, tone, domain, color world, review bar, and cross-domain lens (cinema, architecture, marketing, UX, automotive, industrial design). See [design-thinking/SKILL.md](https://github.com/connerkward/ckw-design-skill/blob/main/design-thinking/SKILL.md).
- **design-system**; Load when implementing: tokens, typography, motion, color semantics, backgrounds. Use when building components, pages, or design systems. See [design-system/SKILL.md](https://github.com/connerkward/ckw-design-skill/blob/main/design-system/SKILL.md).
- **design-spatial**; Load when composing layout: explicit grid + 8-point spacing constraints, visual-weight/balance/alignment, and a render-then-critique vision loop. The fix for "spatial understanding is off"; generated layout that's centered mush, misaligned, or breaks at some widths. See [design-spatial/SKILL.md](https://github.com/connerkward/ckw-design-skill/blob/main/deterministic-design/design-spatial/SKILL.md).
- **design-ux**; Load when auditing USABILITY (not just looks): a UI that "feels off"/"sucks to use", is hard to learn, needs an instruction wall, or any interactive tool/editor/app before shipping. Scores the rendered UI against Nielsen's 10 + interaction heuristics via a SEPARATE fresh-eyes judge → prioritized fix list. Usability ≠ aesthetics. See [design-ux/SKILL.md](https://github.com/connerkward/ckw-design-skill/blob/main/deterministic-design/design-ux/SKILL.md).
- **design-philosophy**; Load for high-concept work, campaigns, or when the user asks for a visual philosophy, manifesto, or unmistakable art-like aesthetic. See [design-philosophy/SKILL.md](https://github.com/connerkward/ckw-design-skill/blob/main/design-philosophy/SKILL.md).
## Visual assets: generate or source
## LLM-assisted work: always annotate model + cost
## Algorithm / model explainers: show the equation, annotate the terms
with its key terms annotated**; don't settle for prose. A scorer described only in
- Render one clean, central equation per algorithm; the "sexy" core, not every
- Keep it dependency-light; styled HTML/Unicode math is fine and works offline; only
> • **e**ₓ; the image's 768-d embedding · **w**, b; weights learned from your labels
## Select-all always has a deselect: no dead-end selections
Any **"Select all"** affordance MUST be paired with a way to **clear the selection** -
the page, to get back. The cost is silent; it only bites *after* they've committed to
  *filtered*, all *on this page*); don't let "Select all" grab 500 but "Clear" only
  check-all) needs its inverse one tap away. Symmetry of action; see