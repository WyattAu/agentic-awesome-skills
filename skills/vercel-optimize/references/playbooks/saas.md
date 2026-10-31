Function Duration dominates (every dashboard request runs the function fully; no edge caching for auth-gated content). Edge Requests grow with API surface. ISR rarely applies. Image Optimization rarely material.
- **Sequential session+permission checks.** `await getSession()` then `await checkPermissions()` then `await loadData()`; do not fetch private data ahead of its authorizing session/permission guard; combine an authorized query or preserve the dependency.
- `vercel-react-best-practices:server-cache-react`; per-request dedup
- `vercel-react-best-practices:server-parallel-fetching`; restructure for Promise.all
- `vercel-react-best-practices:async-suspense-boundaries`; stream the dashboard shell
- `vercel-react-best-practices:server-after-nonblocking`; defer audit/analytics writes (Next 15+)
- `vercel-react-best-practices:client-swr-dedup`; replace polling with SWR
- `https://vercel.com/docs/fluid-compute`; when cold starts hurt