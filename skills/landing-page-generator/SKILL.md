Generate high-converting landing pages from a product description. Output complete Next.js/React components with multiple section variants, proven copy frameworks, SEO optimization, and performance-first patterns. Not lorem ipsum; actual copy that converts.
1. **Gather inputs**; collect product name, tagline, audience, pain point, key benefit, pricing tiers, design style, and copy framework using the trigger format below. Ask only for missing fields.
2. **Analyze brand voice** (recommended); if the user has existing brand content (website copy, blog posts, marketing materials), run it through `marketing-skill/content-production/scripts/brand_voice_analyzer.py` to get a voice profile (formality, tone, perspective). Use the profile to inform design style and copy framework selection:
3. **Select design style**; map the user's choice (or infer from brand voice analysis) to one of the four Tailwind class sets in the Design Style Reference.
4. **Apply copy framework**; write all headline and body copy using the chosen framework (PAS / AIDA / BAB) before generating components. Match the voice profile's formality and tone throughout.
5. **Generate sections in order**; Hero → Features → Pricing → FAQ → Testimonials → CTA → Footer. Skip sections not relevant to the product.
6. **Validate against SEO checklist**; run through every item in the SEO Checklist before outputting final code. Fix any gaps inline.
7. **Output final components**; deliver complete, copy-paste-ready TSX files with all Tailwind classes, SEO meta, and structured data included.
- *Example; H1:* "Your team wastes 3 hours a day on manual reporting" / *Sub:* "Every hour spent on spreadsheets is an hour not closing deals. Your competitors are already automated." / *CTA:* "Automate your reports in 10 minutes →"
## Representative Component: Hero (Centered Gradient: Dark SaaS)
> **Validation step:** Before outputting final code, verify every checklist item above is satisfied. Fix any gaps inline; do not skip items.
- Hero image not preloaded; add `priority` prop to first `<Image>`
- Missing mobile breakpoints; always design mobile-first with `sm:` prefixes
- CTA copy too vague; "Get started" beats "Learn more"; "Start free trial" beats "Sign up"
- Pricing page missing trust signals; add money-back guarantee and testimonials near CTA
- No above-the-fold CTA on mobile; ensure button is visible without scrolling on 375px viewport
- **Brand Voice Analyzer** (`marketing-skill/content-production/scripts/brand_voice_analyzer.py`); Run before generation to establish voice profile and ensure copy consistency
- **UI Design System** (`product-team/ui-design-system/`); Generate design tokens from brand color before building the page
- **Competitive Teardown** (`product-team/competitive-teardown/`); Competitive positioning informs landing page messaging and differentiation