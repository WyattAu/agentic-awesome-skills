1. **Think in types, not pages.** A "Landing Page" is a content type with fields; not an HTML file. This lets you reuse components across pages.
3. **Design for reuse.** If testimonials appear on 5 pages, create a Testimonial type and reference it; don't duplicate.
- `metaTitle`; 50-60 characters
- `metaDescription`; 150-160 characters
- `ogImage`; 1200x630px social preview
- `slug`; URL path segment
- `canonicalUrl`; optional override
- `noIndex`; boolean for excluding from search
- `structuredData`; optional JSON-LD override
1. **Draft**; Author creates or edits content
2. **Review**; Editor reviews for accuracy, brand voice, SEO
3. **Approve**; Stakeholder signs off
4. **Schedule**; Set publish date/time
5. **Publish**; Content goes live via API
- **Hygraph**; GraphQL-native, strong for federation and multi-source content
- **Keystatic**; Git-based, good for developer-content hybrid workflows
- **Payload**; TypeScript-first, self-hosted, code-configured like Sanity
- **Builder.io**; Visual editor with headless backend, good for non-technical marketers
- **Prismic**; Slice-based content modeling, strong Next.js integration
- [Sanity documentation](https://www.sanity.io/docs); GROQ queries, mutations, CLI
- [Contentful documentation](https://www.contentful.com/developers/docs/); Delivery/Management APIs, publishing
- [Strapi documentation](https://docs.strapi.io/); REST CRUD, filters, document API