| 1 | **Shared element** (`name`) | "Same thing; going deeper" |
| Revalidation / background refresh | `default="none"` | Silent; no animation needed |
Reserve directional slides for hierarchical navigation (list → detail) and ordered sequences (prev/next photo, carousel, paginated results). For ordered sequences, the direction communicates position: "next" slides from right, "previous" from left. Lateral/unordered navigation (tab-to-tab) should not use directional slides; it falsely implies spatial depth.
When adding view transitions to an existing app, **follow `references/implementation.md` step by step.** Start with the audit; do not skip it. Copy the CSS recipes from `references/css-recipes.md` into the global stylesheet; do not write your own animation CSS.
// Broken; div wraps the VT, suppressing enter/exit
- `::view-transition-old(.class)`; outgoing snapshot
- `::view-transition-new(.class)`; incoming snapshot
- `::view-transition-group(.class)`; container
- `::view-transition-image-pair(.class)`; old + new pair
Tag transitions with `addTransitionType` so VTs can pick different animations based on context. Call it multiple times to stack types; different VTs in the tree react to different types:
Same `name` on two VTs; one unmounting, one mounting; creates a shared element morph:
// On the other view; same name
- Only one VT with a given `name` can be mounted at a time; use unique names (`photo-${id}`). Watch for reusable components: if a component with a named VT is rendered in both a modal/popover *and* a page, both mount simultaneously and break the morph. Either make the name conditional (via a prop) or move the named VT out of the shared component into the specific consumer.
- Never use a fade-out exit on pages with shared morphs; use a directional slide instead.
Shared elements and list identity are independent concerns; don't confuse one for the other. When a list item contains a shared element (e.g., an image that morphs into a detail view), use two nested `<ViewTransition>` boundaries:
Without it, every VT fires the browser cross-fade on **every** transition; Suspense resolves, `useDeferredValue` updates, background revalidations. Always use `default="none"` and explicitly enable only desired triggers.
**Pattern A; Directional slides:** Type-keyed VT on each page, fires during navigation.
**Pattern B; Suspense reveals:** Simple string props, fires when data loads (no type).
When a parent VT exits, nested VTs inside it do **not** fire their own enter/exit; only the outermost VT animates. Per-item staggered animations during page navigation are not possible today. See [react#36135](https://github.com/facebook/react/pull/36135) for an experimental opt-in fix.
- **`references/implementation.md`**; Step-by-step implementation workflow.
- **`references/patterns.md`**; Patterns, animation timing, events API, troubleshooting.
- **`references/css-recipes.md`**; Ready-to-use CSS animation recipes.
- **`references/nextjs.md`**; Next.js App Router patterns and Server Component details.