This wraps every `<Link>` navigation in `document.startViewTransition`. Any VT with `default="auto"` fires on **every** link click; use `default="none"` to prevent competing animations.
Do **not** install `react@canary`; see SKILL.md "Availability" for details.
**Step 4:** Use `transitionTypes` on `<Link>`; see "The `transitionTypes` Prop" section below for usage and availability.
**After Step 6:** For same-route dynamic segments (e.g., `/collection/[slug]`), use the `key` + `name` + `share` pattern; see Same-Route Dynamic Segment Transitions below.
**Do NOT add a layout-level VT wrapping `{children}` if pages have their own VTs.** Nested VTs never fire enter/exit when inside a parent VT; page-level enter/exit will silently not work. Remove the layout VT entirely.
**Layouts persist across navigations**; `enter`/`exit` only fire on initial mount, not on route changes. Don't use type-keyed maps in layouts.
**Availability:** `transitionTypes` requires `experimental.viewTransition: true` and is available in Next.js 15+ canary builds and Next.js 16+. If unavailable, use `startTransition` + `addTransitionType` + `router.push()` (see Programmatic Navigation below). To check: `grep -r "transitionTypes" node_modules/next/dist/`; if no results, fall back to programmatic navigation.
// Detail page; same name
When navigating between dynamic segments of the same route (e.g., `/collection/[slug]`), the page stays mounted; enter/exit never fire. Use `key` + `name` + `share`:
- `<Link transitionTypes>` works in Server Components; no `'use client'` needed