5. **Discovery surfaces are generated from content.** `sitemap.xml`, `robots.txt`, and the RSS feed are built from the same content collections the app renders; never maintained by hand, never drifting.
**route files never build SEO data inline**; they call a builder and mount the result.
export const SITE_TITLE_DEFAULT = "Jane Doe; Senior Engineer";
(here, unprefixed). Keep whichever convention the host project already uses; consistency wins.
- Never concatenate `SITE_URL + path` by hand; always `canonicalUrl(path)`.
// services/seo/index.ts  (Next.js example; swap the return type for other frameworks)
// app/layout.tsx; global metadata, all values from constants/seo
- Every page resolves a single `canonical`; never emit duplicate or relative canonicals.
// app/sitemap.ts; thin adapter
// app/feed.xml/route.ts; sort newest-first, CDATA-wrap free text
(`{ title, description, canonical, ogImage }`) and let each adapter translate it; keep the
- [ ] All site identity (URL, name, description, keywords, author, handles, OG image, verification) lives in **one** `constants/seo` module; no duplicates, no hardcoded base URL.
- [ ] Every absolute URL is produced by `canonicalUrl()`; no manual `SITE_URL + path`.
2. Keep the frontmatter `name` and high-signal `description`; discovery indexes match against it.