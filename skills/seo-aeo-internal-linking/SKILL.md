      "title": "string; page or post title",
      "url": "string; URL if published, leave blank if not yet live",
      "primary_keyword": "string; the keyword this page targets",
      "content_summary": "string; 1-2 sentences describing the page"
  "focus_page": "string; title of the page you most want to
  "cluster_context": "string; optional, paste content cluster
  "product_pages": ["string; product, signup, pricing, or demo pages that should receive relevant CTAs"],
  "audit_findings": "string; optional orphan, cannibalization, or priority findings"
| Cluster → Pillar        | Cluster post links back to pillar  | Consolidates authority upward; highest priority |
Pages with no incoming internal links; these may be harder to discover, but can still be found through sitemaps or external links; confirm crawl scope before calling them site-wide orphans:
Listed in priority order; highest impact first.
### 🔴 High Priority: Do These First
| Placement          | [Where in the source page; intro / body / CTA]   |
### 🟡 Medium Priority: Do These Second
### 🟢 Low Priority: Polish Round
    ├── [Cluster Article 1]; anchor: [text]
    ├── [Cluster Article 2]; anchor: [text]
    └── [Cluster Article 3]; anchor: [text]
    ├── [Cluster Article 1]; anchor: [text]
    └── [Cluster Article 2]; anchor: [text]
    [Article 1] ↔ [Article 3]; anchor: [text]
    [Article 2] → [Article 4]; anchor: [text]
1. Read `cluster_context` if provided; use it to understand
2. Index all pages by `page_type`; pillar pages should receive
3. Run orphan detection; flag any page with zero incoming
4. Build semantic overlap matrix; match pages by
6. Write a context sentence for every suggestion -
7. Check anchor text across all suggestions; flag any
10. Run the Links Per Page check; flag any page
11. Check product-page paths separately; confirm each