description: "Build full-stack web applications with SvelteKit; file-based routing, SSR, SSG, API routes, and form actions in one framework."
SvelteKit is the official full-stack framework built on top of Svelte. It provides file-based routing, server-side rendering (SSR), static site generation (SSG), API routes, and progressive form actions; all with Svelte's compile-time reactivity model that reduces some runtime work through compilation; client-side features still ship runtime JavaScript. Use this skill when building fast, modern web apps where both DX and performance matter.
Form actions are the SvelteKit-native way to handle mutations; no client-side fetch required:
export const prerender = true;   // Static; generated at build time
export const ssr = true;         // Default; rendered on server per request
### Example 2: Hooks: Session Middleware
- ✅ Use `+page.server.ts` for database/auth logic; it never ships to the client
- ✅ Use form actions for mutations instead of client-side `fetch`; works without JS
- ❌ Don't store sensitive state in stores; use `locals` on the server
- ❌ Don't skip `use:enhance` on forms; without it, forms lose progressive enhancement
- All code in `+page.server.ts`, `+server.ts`, and `$lib/server/` runs exclusively on the server; safe for DB queries, secrets, and session validation.
- CSRF protection is built-in for form actions; do not disable `checkOrigin` in production.
- `@nextjs-app-router-patterns`; When you prefer React over Svelte for SSR/SSG
- `@trpc-fullstack`; Add end-to-end type safety to SvelteKit API routes
- `@auth-implementation-patterns`; Authentication patterns usable with SvelteKit hooks
- `@tailwind-patterns`; Styling SvelteKit apps with Tailwind CSS