description: "Build ultra-fast web APIs and full-stack apps with Hono; runs on Cloudflare Workers, Deno, Bun, Node.js, and any WinterCG-compatible runtime."
Hono (炎, "flame" in Japanese) is a small, ultrafast web framework built on Web Standards (`Request`/`Response`/`fetch`). It runs anywhere: Cloudflare Workers, Deno Deploy, Bun, Node.js, AWS Lambda, and any WinterCG-compatible runtime; with the same code. Hono's router is one of the fastest available, and its middleware system, built-in JSX support, and RPC client make it a strong choice for edge APIs, BFFs, and lightweight full-stack apps.
Hono middleware works exactly like `fetch` interceptors; before and after handlers:
Hono's RPC mode exports route types that the `hc` client consumes; similar to tRPC but using fetch conventions:
// Fully typed; autocomplete on routes, params, and responses
- ✅ Use route groups (sub-apps) to keep handlers in separate files; `app.route('/users', usersRouter)`
- ❌ Don't add heavy dependencies; Hono's value is its tiny footprint on edge runtimes
- ❌ Don't skip middleware typing; use generics (`Variables`, `Bindings`) to keep `c.get()` type-safe
- For Cloudflare Workers, store non-secret configuration in `wrangler.toml` `[vars]` and secrets with `wrangler secret put`; never hardcode them in source.
- When using `bearerAuth` or `jwt`, ensure tokens are validated server-side; do not trust client-provided user IDs.
- **Problem:** Handler returns `undefined`; response is empty
- **Problem:** Route not matching; gets a 404
- `@cloudflare-workers-expert`; Deep dive into Cloudflare Workers platform specifics
- `@trpc-fullstack`; Alternative RPC approach for TypeScript full-stack apps
- `@zod-validation-expert`; Detailed Zod schema patterns used with `@hono/zod-validator`
- `@nodejs-backend-patterns`; When you need a Node.js-specific backend (not edge)