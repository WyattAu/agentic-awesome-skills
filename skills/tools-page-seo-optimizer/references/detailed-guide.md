## Phase 0: Codebase Reconnaissance
**Before writing any code**, locate these in the codebase. Names vary by framework; adapt.
1. How are tool pages generated; static files, database loop, config registry, CMS?
## Phase 1: Meta Titles & Descriptions
{Tool Name} | {Specific Outcome}; {Brand}
| `Meta Tag Generator \| Create Perfect SEO Titles Free; MySite` | `Meta Tag Generator - MySite Tools` |
| `Broken Link Finder \| Scan Any Page for Dead URLs; MySite` | `Broken Link Finder - Free Online Tool \| MySite` |
- Every tool has a **unique** title; no two tools share the same one
{What it does; one action sentence}. {Key differentiator}. {CTA}.
- Every tool gets a **custom** description; zero template filler
### Validation script: run before every deploy
## Phase 2: Content Registry (The Highest-Leverage Fix)
# Find shared prose in your templates: if these strings appear in a shared template
# data/tools/meta-tag-generator.yaml  (or JSON, DB columns, JS object: adapt to your stack)
  The meta tag generator creates the two most critical on-page SEO elements -
  your title tag and meta description; with live character counters that enforce
  - "Write a unique description per page; duplicate descriptions waste crawl budget"
    a: "No; Google may choose a query-dependent snippet from page content. Write them anyway for
## Phase 3: H1 and Heading Hierarchy
- One `<h1>` per page; only the tool name/title
h1; Tool name (one per page)
  h2; Major sections: "How It Works", "Best Practices", "FAQs", "Related Tools"
    h3; Subsections: individual FAQ items, feature callouts, step headers
## Phase 4: Accessibility
## Phase 5: Internal Linking
- No orphan tools; every tool reachable within 3 clicks from homepage
# Orphan detection: tools with too few inbound references
  <a href="/tools/{{ slug }}">{{ rel.name }}; {{ rel.tagline }}</a>
## Phase 6: URL Slug Hygiene
**Formula:** `{primary-keyword-phrase}`; lowercase, hyphens, no stop words, no "free" / "online" / "tool" padding.
# Audit: list longest slugs (likely stuffed)
## Phase 7: E-E-A-T Signals
  <div>✅ <strong>100% Free</strong>; no account, no credit card</div>
  <div>🔒 <strong>Privacy First</strong>; your data never leaves your browser</div>
  <div>⚡ <strong>Instant Results</strong>; processed in under 1 second</div>
## Phase 8: Scaling to 100+ Tools
## Phase 9: Blog Content Strategy (Position 50–68 Keywords)
at position 50–68; too deep to get clicks; because tool pages aren't the right content
Post title: {Informational keyword}; {Year} Guide
H1: {Target keyword}; the exact GSC query, naturally phrased
  H2: How to {action}; step by step
  H2: {Tool name}; try it free   ← internal link to your tool
- `meta_title`: include year where relevant ("2025"); improves CTR on informational queries
- `datePublished` + `dateModified` in schema; critical for freshness signals
<!-- Good; inline contextual link -->
<!-- Weak; sidebar only, no body link -->
## Phase 10: Live Deployment Verification
the live site; not just the build.
If any check fails; **do not move on**. Diagnose and fix before the next phase.
## Phase 11: Pre-Commit Validation
# Quick check: meta validation + live site verification
# Full check: quick + duplicate title check
- [schema-markup-generator](../../schema-markup-generator/SKILL.md); JSON-LD structured data (HowTo, FAQPage, WebApplication) for tool pages
- [social-metadata-hardening](../../social-metadata-hardening/SKILL.md); OG tags and social sharing previews for tool pages
- [indexing-issue-auditor](../../indexing-issue-auditor/SKILL.md); full crawl audit and redirect mapping after slug changes
- [pagespeed-enhancer](../../pagespeed-enhancer/SKILL.md); Lighthouse / Core Web Vitals audit for tool pages
- [wordpress-centric-high-seo-optimized-blogwriting-skill](../../wordpress-centric-high-seo-optimized-blogwriting-skill/SKILL.md); blog post writing with SEO structure
- [vibecode-production-qa-validator](../../vibecode-production-qa-validator/SKILL.md); end-to-end production QA including deployment verification