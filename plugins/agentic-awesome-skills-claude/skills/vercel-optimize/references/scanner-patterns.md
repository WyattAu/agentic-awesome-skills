### `cache-components-suspense-dedupe`: 'use cache' with multiple Suspense boundaries on the same data
### `edge-heavy-import`: Heavy / node-only import inside edge-runtime file
### `force-dynamic`: export const dynamic = 'force-dynamic'
**Fix.** Audit the route. If dynamic behavior comes from cookies()/headers()/searchParams, force-dynamic may be redundant; Next infers dynamic automatically. Consider revalidate / 'use cache' / generateStaticParams if any portion can be pre-rendered.
### `headers-in-page`: Dynamic API call forcing dynamic rendering
### `large-static-asset`: Large file in public/
**Description.** Static assets in `public/` over 500 KB ship as-is from the CDN. Whether the cost is meaningful depends on traffic, but the candidate is binary; the file is either needed at that size or it can be optimized (compressed image, video transcode, or moved off the critical path).
### `max-age-without-s-maxage`: Cache-Control: max-age without s-maxage
### `middleware-broad-matcher`: Middleware matcher missing or too broad
### `missing-cache-headers`: Cacheable route or fetch with no caching (Cache-Control absent or no-store)
### `prisma-include-tree-bloat`: Deep Prisma include tree (3+ levels)
### `region-pin-in-config`: Function region pinned in config
### `source-maps-production`: Source maps enabled in production
### `sveltekit-prerender-missing`: SvelteKit page without explicit prerender / ISR config
**Description.** SvelteKit page or +page.server.ts is missing an explicit `prerender`, `ssr`, or adapter `config.isr` declaration. Default is per-request function execution; investigate whether the route could be prerendered or ISR-cached.
### `turbo-force-bypass`: Turborepo cache bypass on a monorepo
### `unoptimized-image`: Image optimization gap (raw <img>, global flag, missing sizes, or SVG mis-routed)
### `use-cache-date-stamp`: new Date() / Date.now() / Math.random() inside a 'use cache' file