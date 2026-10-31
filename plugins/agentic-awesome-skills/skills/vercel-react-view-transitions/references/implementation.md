- **Every `<Link>` and `router.push`**; these are your navigation triggers. Open every file that contains one.
- **Every `<Suspense>` boundary**; each one is a candidate for a reveal animation. Check what its fallback renders.
- **Every page/route component**; list them all. Each page needs a VT placement decision.
- **Persistent elements**; headers, navbars, sidebars, sticky controls that stay on screen across navigations. These need `viewTransitionName` isolation.
- **Shared visual elements**; images, cards, or avatars that appear on both a source and target view (e.g., a thumbnail in a list and the same image on a detail page).
- **Skeleton-to-content control pairs**; if a Suspense fallback renders a control (search input, tab bar) that also exists in the real content, both need a matching `viewTransitionName`.
| (Suspense)      | (content loads)      |;            | slide-up reveal       |
For each shared element (`name` prop), note every navigation where a pair forms and where it doesn't; this determines whether you need `enter`/`exit` as a fallback alongside `share`.
Do not write your own animation CSS; the recipes handle staggered timing, motion blur on morphs, and reduced motion that are easy to get wrong. You can customize timing variables (`--duration-exit`, `--duration-enter`, `--duration-move`) after the initial setup.
- Always pair `enter` with `exit`; without an exit animation, the old page disappears instantly while the new one animates in.
- Always include `default: "none"` in type map objects and `default="none"` on the component; otherwise it fires on every transition.
This example uses `slide-down` / `slide-up` for directional vertical motion. For a simpler reveal, a bare `<ViewTransition>` around the `<Suspense>` gives a cross-fade with zero configuration. Choose based on the spatial meaning; consult the "Choosing the Right Animation Style" table in the main skill file.
- Use simple string props (not type maps) on Suspense `<ViewTransition>`s; Suspense resolves fire as separate transitions with no type, so type-keyed props won't match.
// On the target view (e.g., detail page); same name
When list items contain shared elements, compose both patterns with two nested `<ViewTransition>` layers; see "Composing Shared Elements with List Identity" in `SKILL.md`.
- Names must be globally unique; use prefixes like `photo-${id}`.
- **Bare `<ViewTransition>` without props**; without `default="none"`, it fires the browser's default cross-fade on every transition (every navigation, every Suspense resolve, every revalidation). Always set `default="none"` and explicitly enable only the triggers you want.
- **Directional `<ViewTransition>` in a layout**; layouts persist across navigations and never unmount/remount. `enter`/`exit` props won't fire on route changes. Place the outer type-keyed `<ViewTransition>` in each page component.
- **Fade-out exit with shared element morphs**; the page dissolving conflicts with the morph. Use a directional slide exit instead.
- **Writing custom animation CSS**; the recipes in `css-recipes.md` handle staggered timing, motion blur on morphs, and reduced motion. Copy them; don't reinvent them.
- **Missing `default: "none"` in type-keyed objects**; TypeScript requires a `default` key, and without it the fallback is `"auto"` which fires on every transition.
- **Type maps on Suspense reveals**; Suspense resolves fire as separate transitions with no type. Type-keyed props won't match; use simple string props instead.
- **Raw `viewTransitionName` CSS to trigger animations**; React only calls `document.startViewTransition` when `<ViewTransition>` components are in the tree. A bare `viewTransitionName` style is for isolating elements from a parent's snapshot, not for triggering animations.
- **`update` trigger for same-route navigations**; nested VTs inside the content steal the mutation from the parent, so `update` never fires on the outer VT. Use `key` + `name` + `share` instead.
- **Named VT in a reusable component**; if a component with a named VT is rendered in both a modal/popover *and* a page, both mount simultaneously and break the morph. Make the name conditional or move it to the specific consumer.
- **`router.back()` for back navigation**; `router.back()` triggers synchronous `popstate`, incompatible with view transitions. Use `router.push()` with an explicit URL.