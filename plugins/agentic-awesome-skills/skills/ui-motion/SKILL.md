description: "Apply a named StyleSeed motion to a component; either one of the 5 personality seeds (Spring/Silk/Snap/Float/Pulse × entrance/exit/hover/press/layout) or a distinctive keyword move from the motion library (toggle-flip, toggle-curtain, reveal-blur, pop-in, shimmer, …)."
- For non-React motion (CSS-only transitions, GSAP); this skill targets `motion.X` JSX only
- For full scroll-linked timelines or parallax; out of scope per DESIGN-LANGUAGE.md Rule 59
- For tweaking the existing FadeIn/FadeUp/Stagger wrappers; edit `engine/components/ui/motion.tsx` directly
## Recommend mode: use-case → motion (when the user describes the *moment*, not the vibe)
| Primary button / CTA press | `spring · press` | tactile, confident; the press should "give" |
| Dropdown / popover / menu | `snap · entrance` | instant, precise; frequent UI shouldn't wait |
| Dashboard / data card hover | `snap · hover` | a subtle lift only; keep dense UI calm |
| Number / balance / KPI / price reveal | **none** | don't animate the payload; it must read instantly |
1. **One seed per product.** If the project already uses a seed, match it; don't introduce a second personality.
`engine/motion/library.ts` adds *distinctive moves*; a flip, a curtain wipe, a
morph; each behind a unique keyword. Prefer a keyword when the user wants a
`@engine/motion`) is the **single source of truth**; every keyword carries its
1. Read the exact recipe from `engine/motion/library.ts`; find the entry whose
from the table; never invent a keyword.
1. **Read the target file** at the path given (or, if no path was given, ask the user which file). Locate the JSX element the user is talking about; usually a `<button>`, `<div>`, `<Card>`, or similar.
   - the chosen seed from `"@engine/motion"`; in a project that doesn't use the `@engine/*` alias, use a relative path to `engine/motion`
4. **Do NOT inline the params**. The whole point of the seed is that the values come from one source. Never expand `{ type: "spring", stiffness: 300, damping: 18 }` into the JSX; always spread the recipe.
- Do not edit `engine/motion/seeds/*.ts` from this skill; those are calibrated by hand. Add a new seed only via a separate, explicit ask.