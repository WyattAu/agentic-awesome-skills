Edge Requests dominate. Image Optimization is high (hero images, illustrations, product screenshots). Bandwidth matters for video content. Function Duration is usually low; most pages are static or ISR.
3. **ISR for content-driven sections.** Customer logos, testimonials, "latest blog post" widgets, pricing tables; anything coming from a CMS. Revalidate hourly or on webhook.
- `https://vercel.com/docs/bot-management`; require observed abusive traffic and staged validation
- `https://vercel.com/docs/incremental-static-regeneration`; for CMS-driven sections
- `https://nextjs.org/docs/app/api-reference/components/image`; hero/illustration optimization
- `vercel-react-best-practices:bundle-defer-third-party`; defer analytics/pixels
- `https://nextjs.org/docs/app/building-your-application/routing/middleware`; A/B variant routing at the edge