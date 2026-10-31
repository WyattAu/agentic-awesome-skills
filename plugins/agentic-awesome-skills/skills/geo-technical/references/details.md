- Count URLs; compare to expected number of indexable pages
- Check for canonical chains (A canonicals to B, B canonicals to C; should be A to C)
- Flat where appropriate; avoid unnecessarily deep nesting
| **LCP** (Largest Contentful Paint) | < 2.5s | 2.5s - 4.0s | > 4.0s | Measures loading; time until largest visible element renders |
| **CLS** (Cumulative Layout Shift) | < 0.1 | 0.1 - 0.25 | > 0.25 | Measures visual stability; unexpected layout movements |
## Category 7: Server-Side Rendering (15 points): CRITICAL FOR GEO
These checks surface emerging AI agent compatibility signals. None contribute to the numeric score; they produce a pass or a recommendation. The underlying standards are either IETF drafts or early-adoption features; penalizing absence would be unfair.
RFC 8288 (Web Linking) defines the HTTP `Link:` response header. Servers can use it to advertise related resources; API catalog, service docs, MCP server card; in a machine-readable way, without HTML parsing.
**When to surface a recommendation:** Only for API-first sites (API docs linked in nav, `/api/` or `/developers/` paths, swagger/OpenAPI in sitemap). Omit this section entirely for standard business sites; absence is expected and not noteworthy.
| `Link:` headers present, known rel types | Informational; document what was found |
| `Link:` headers present, unknown rel types | Informational; note and explain |
| Absent, API-first site | Recommendation; explain and suggest implementation |
| Absent, standard business site | Omit; do not surface |
Checks if the server responds to `Accept: text/markdown` with `Content-Type: text/markdown`. Cloudflare's "Markdown for Agents" feature enables this; AI agents receive clean Markdown instead of HTML, eliminating boilerplate stripping and improving content extraction accuracy.
- If response `Content-Type` is `text/markdown` (or `text/markdown; charset=utf-8`): pass; note as a leading-edge capability.
| `text/markdown` returned | Bonus; note as a leading-edge capability |
- **90-100**: Excellent; technically sound for both traditional SEO and GEO
- **70-89**: Good; minor issues to address but fundamentally solid
- **50-69**: Needs Work; significant technical debt impacting visibility
- **30-49**: Poor; major issues blocking crawling, indexing, or AI visibility
- **0-29**: Critical; fundamental technical failures requiring immediate attention
# GEO Technical SEO Audit: [Domain]