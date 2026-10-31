# Stitch Design Taste: Semantic Design System Skill
This skill generates `DESIGN.md` files optimized for Google Stitch screen generation. It translates the battle-tested anti-slop frontend engineering directives into Stitch's native semantic design language; descriptive, natural-language rules paired with precise values that Stitch's AI agent can interpret to produce premium, non-generic interfaces.
1. **Visual atmosphere**; the mood, density, and design philosophy
2. **Color calibration**; neutrals, accents, and banned patterns with hex codes
3. **Typographic architecture**; font stacks, scale hierarchy, and anti-patterns
4. **Component behaviors**; buttons, cards, inputs with interaction states
5. **Layout principles**; grid systems, spacing philosophy, responsive strategy
6. **Motion philosophy**; animation engine specs, spring physics, perpetual micro-interactions
7. **Anti-patterns**; explicit list of banned AI design clichés
- The "AI Purple/Blue Neon" aesthetic is strictly BANNED; no purple button glows, no neon gradients
- Stick to one palette for the entire output; no warm/cool gray fluctuation
- Never use pure black (`#000000`); use Off-Black, Zinc-950, or Charcoal
- **Loading States:** Skeletal loaders matching layout dimensions; no generic circular spinners
- No overlapping elements; every element occupies its own clear spatial zone. No absolute-positioned content stacking
- Centered Hero sections are BANNED when variance exceeds 4; force Split Screen, Left-Aligned, or Asymmetric Whitespace
- The generic "3 equal cards horizontally" feature row is BANNED; use 2-column Zig-Zag, asymmetric grid, or horizontal scroll
- CSS Grid over Flexbox math; never use `calc()` percentage hacks
- Full-height sections must use `min-h-[100dvh]`; never `h-screen` (iOS Safari catastrophic jump)
- **Spring Physics default:** `stiffness: 100, damping: 20`; premium, weighty feel. No linear easing
- No generic serif fonts (`Times New Roman`, `Georgia`, `Garamond`); distinctive modern serifs only if needed
- No overlapping elements; clean spatial separation always
- No broken Unsplash links; use `picsum.photos` or SVG avatars
and fluid spring-physics motion. The atmosphere is clinical yet warm; like a
- **Canvas White** (#F9FAFB); Primary background surface
- **Pure Surface** (#FFFFFF); Card and container fill
- **Charcoal Ink** (#18181B); Primary text, Zinc-950 depth
- **Muted Steel** (#71717A); Secondary text, descriptions, metadata
- **Whisper Border** (rgba(226,232,240,0.5)); Card borders, 1px structural lines
- **[Accent Name]** (#XXXXXX); Single accent for CTAs, active states, focus rings
- **Display:** [Font Name]; Track-tight, controlled scale, weight-driven hierarchy
- **Body:** [Font Name]; Relaxed leading, 65ch max-width, neutral secondary color
- **Mono:** [Font Name]; For code, metadata, timestamps, high-density numbers
* **Empty States:** Composed, illustrated compositions; not just "No data" text.
- **Be Descriptive:** "Deep Charcoal Ink (#18181B)"; not just "dark text"
- **Be Opinionated:** This is not a neutral template; it enforces a specific, premium aesthetic
1. Start with the atmosphere; understand the vibe before detailing tokens
2. Look for patterns; identify consistent spacing, sizing, and styling
3. Think semantically; name colors by purpose, not just appearance
4. Consider hierarchy; document how visual weight communicates importance
5. Encode the bans; anti-patterns are as important as the rules themselves
- Ignoring the anti-pattern list; these are what make the output premium