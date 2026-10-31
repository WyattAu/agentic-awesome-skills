 ; detect, validate, and generate JSON-LD markup
Look for `<script type="application/ld+json">` blocks in the HTML. Parse each block as JSON. A page may contain multiple JSON-LD blocks; collect all of them.
Look for elements with `typeof`, `property`, and `vocab` attributes. Similar to Microdata; recommend migration to JSON-LD.
### Organization (CRITICAL: every business site)
**Status as of 2024**: Google restricts FAQ rich results to government and health sites. However, the FAQPage schema still serves GEO purposes; AI platforms parse FAQ structured data for question-answer extraction. Implement it for AI readability even though rich results may not appear.
### Person (standalone: for personal brands, authors, thought leaders)
1. **Wikipedia article**; highest authority entity link
2. **Wikidata item**; machine-readable entity identifier (e.g., `https://www.wikidata.org/wiki/Q12345`)
3. **LinkedIn**; company page or personal profile
4. **YouTube**; channel URL
5. **Twitter/X**; profile URL
6. **Facebook**; page URL
7. **Crunchbase**; company profile (for startups/tech)
8. **GitHub**; organization or personal profile (for tech)
9. **Google Scholar**; author profile (for researchers/academics)
10. **ORCID**; researcher identifier (for academics)
11. **Instagram**; profile URL
12. **Apple App Store / Google Play**; app listings (for software)
13. **BBB**; Better Business Bureau listing (for US businesses)
14. **Industry directories**; relevant vertical directories
1. **Organization or Person** (depending on entity type); always
3. **Business-type-specific**; Article for publishers, Product for e-commerce, LocalBusiness for local, SoftwareApplication for SaaS
4. **BreadcrumbList**; for any page deeper than homepage
# GEO Schema & Structured Data Report: [Domain]