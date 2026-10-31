description: "Build content-focused websites with Astro; zero JS by default, islands architecture, multi-framework components, and Markdown/MDX support."
Astro is a web framework designed for content-rich websites; blogs, docs, portfolios, marketing sites, and e-commerce. Its core innovation is the **Islands Architecture**: by default, Astro ships zero JavaScript to the browser. Interactive components are selectively hydrated as isolated "islands." Astro supports React, Vue, Svelte, Solid, and other UI frameworks simultaneously in the same project, letting you pick the right tool per component.
// This block runs on the server ONLY; never in the browser
### Step 5: Islands: Selective Hydration
<!-- Static HTML; no JavaScript sent to browser -->
<!-- Hydrated immediately; this island is interactive -->
- ✅ Keep most components as static `.astro` files; only hydrate what must be interactive
- ✅ Use content collections for all Markdown/MDX content; you get type safety and auto-validation
- ✅ Use `import.meta.env` for environment variables; prefix public vars with `PUBLIC_`
- ❌ Don't use `client:load` on every component; this defeats Astro's performance advantage
- ❌ Don't skip `getStaticPaths` for dynamic routes in static mode; builds will fail
- Sanitize any user-supplied content before rendering with `set:html`; it bypasses auto-escaping.
  **Solution:** Add a `client:` directive (`client:load`, `client:visible`, etc.); without it, components render as static HTML only.
  **Solution:** Astro's dev server watches content files; restart if changes to `content/config.ts` are not reflected.
- **Problem:** `Astro.props` type is `any`; no autocomplete
- `@sveltekit`; When you need a full-stack framework with reactive UI (vs Astro's content focus)
- `@nextjs-app-router-patterns`; When you need a React-first full-stack framework
- `@tailwind-patterns`; Styling Astro sites with Tailwind CSS
- `@progressive-web-app`; Adding PWA capabilities to an Astro site