If the question is about logic/state rather than what something looks like; wrong branch. Use [LOGIC.md](LOGIC.md).
## Two sub-shapes: strongly prefer sub-shape A
A UI prototype is much easier to judge when it's **butting up against the rest of the app**; real header, real sidebar, real data, real density. A throwaway route on its own is a vacuum: every variant looks fine in isolation. Default to sub-shape A whenever there's a plausible existing page to host the variants. Only reach for sub-shape B if the prototype genuinely has no nearby home.
### Sub-shape A: adjustment to an existing page (preferred)
The route already exists. Variants are rendered **on the same route**, gated by a `?variant=` URL search param. The existing data fetching, params, and auth all stay; only the rendering swaps. This is the default; pick it unless there's a specific reason not to.
If the prototype is for something that doesn't yet have a page but *would naturally live inside one* (a new section of the dashboard, a new card on the settings screen, a new step in an existing flow); that's still sub-shape A. Mount the variants inside the host page.
### Sub-shape B: a new page (last resort)
Only use this when the thing being prototyped genuinely has no existing page to live inside; e.g. an entirely new top-level surface, or a flow that can't be embedded anywhere sensible.
Create a **throwaway route** following whatever routing convention the project already uses; don't invent a new top-level structure. Name it so it's obviously a prototype (e.g. include the word `prototype` in the path or filename). Same `?variant=` pattern.
Default to **3 variants**. More than 5 stops being radically different and starts being noise; cap there.
Variants must be **structurally different**; different layout, different information hierarchy, different primary affordance, not just different colours. Three slightly-tweaked card grids isn't a UI prototype, it's wallpaper. If two drafts come out too similar, redo one with explicit "do not use a card grid" guidance.
// pseudo-code; adapt to the project's framework
- **Left arrow**; cycles to the previous variant (wraps around).
- **Variant label**; shows the current variant key and, if the variant exports a name, that name too. e.g. `B; Sidebar layout`.
- **Right arrow**; cycles forward (wraps around).
- Clicking an arrow updates the URL search param (use the framework's router; `router.replace` on Next, `navigate` on React Router, etc) so the variant is shareable and reload-stable.
Surface the URL (and the `?variant=` keys). The user will flip through whenever they get to it. The interesting feedback is usually **"I want the header from B with the sidebar from C"**; that's the actual design they want.
- **Sub-shape A**; delete the losing variants and the switcher; fold the winner into the existing page.
- **Sub-shape B**; promote the winning variant to a real route, delete the throwaway route and the switcher.
- **Wiring variants to real mutations.** Read-only prototypes are fine. If a variant needs to mutate, point it at a stub; the question is "what should this look like", not "does the backend work".