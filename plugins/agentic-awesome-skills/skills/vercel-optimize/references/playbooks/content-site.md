Edge Requests dominate (every page view is an edge request; static assets even more). Image Optimization is often the #2 line item. Function Duration tends to be low; most pages should be static or ISR.
- `https://nextjs.org/docs/app/api-reference/functions/generate-static-params`; for pre-rendering
- `https://vercel.com/docs/incremental-static-regeneration`; for the ISR fix
- `https://nextjs.org/docs/app/api-reference/components/image`; image optimization
- `https://nextjs.org/docs/app/api-reference/components/font`; self-hosted fonts
- `vercel-react-best-practices:bundle-defer-third-party`; defer analytics/cookie banners