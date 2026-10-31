## PHASE 1: Ingest & Classify
| Performance     | 80    | ⚠️ Warn | LCP 4.0s; element render delay     |
| Accessibility   | 100   | ✅ Pass |;                                   |
| SEO             | 100   | ✅ Pass |;                                   |
## PHASE 2: Batch Scan (4 Sections)
### Batch A: Performance Scan
| CSS `@import` rules | FCP, LCP | Are external stylesheets loaded via `@import url()` in CSS? This can add a sequential discovery dependency; browser must fetch CSS, parse it, then fetch imported CSS. Use `<link>` instead. |
### Batch B: Accessibility Scan
### Batch C: Best Practices Scan
| Third-party cookies | Any `SameSite=None` cookies without `Secure`? |; | 🟡 Medium |
| Deprecated APIs | Any browser-deprecated JS APIs in use? |; | 🟢 Low |
| Source maps | Are source maps deployed for debugging? |; | 🟢 Low |
### Batch D: SEO Scan
| SPA meta injection | If using react-helmet-async / Next.js Head: verify via "View Page Source", not DevTools Elements; meta tags may be JS-injected |
## PHASE 3: Risk Report
## PHASE 4: Fix Batches
1. **What to change**; file, line, specific change
4. **Expected effect**; hypothesis unless measured
5. **How to verify**; what to check after deploying
### Fix Batch 1: Candidate quick wins (verify before authorized deployment)
**F1.1; Move CSS `@import` to `<link>` tag**
**F1.2; Defer render-blocking CSS (if not above-fold critical)**
**F1.3; Fix broken preconnect (crossorigin mismatch)**
<!-- Before (broken; no crossorigin on font CDN) -->
**F1.4; Convert images to WebP**
**F1.5; Add explicit image dimensions (CLS fix)**
**F1.6; Add security headers (netlify.toml)**
**F1.7; Add security headers (vercel.json)**
**F1.8; Self-host Google Fonts (eliminate external CSS request)**
**F1.9; Resize oversized icons**
### Fix Batch 2: Medium Effort (staging test recommended)
**F2.1; Remove LCP element lazy loading**
<!-- Before: wrong; LCP image is lazy -->
**F2.2; Preload LCP image**
**F2.3; Reduce unused JS (Vite/Rollup config)**
// vite.config.js; enable manual chunking
**F2.4; Eliminate forced reflows**
**F2.5; Optimise DOM size**
### Fix Batch 3: Refactor Required (full QA cycle)
**F3.1; External API in critical path (e.g. api.rss2json.com)**
**F3.2; Content Security Policy (full CSP)**
□ npm run build (or equivalent); exits 0
□ npm run lint / typecheck; no new errors vs baseline
  (react-helmet-async injects at runtime; check prerendered/SSR output)
## PHASE 5: Verification Checklist
- Never suggest fixes that conflict with each other; sequence matters
| F1.1; CSS @import → `<link>` | `index.html`, `src/styles/*.css` | □ Yes □ No | | Restore original `<link>` tags |
| F1.2; Defer render-blocking CSS | `index.html` | □ Yes □ No | | Remove `media="print"` + `onload` |
| F1.4; WebP conversion | `public/images/*.webp` | □ Yes □ No | | Delete .webp files, restore originals |
| F1.5; Image dimensions | `src/components/*.tsx` | □ Yes □ No | | Remove `width`/`height`/`loading` attrs |
| F1.6; Security headers (Netlify) | `netlify.toml` | □ Yes □ No | | Delete the `[[headers]]` block |
| F1.7; Security headers (Vercel) | `vercel.json` | □ Yes □ No | | Remove the `"headers"` array entry |
| F1.8; Self-host fonts | `public/fonts/*.woff2`, `src/styles/fonts.css`, `index.html` | □ Yes □ No | | Delete font files, remove `@font-face`, restore Google Fonts `<link>` |
| F1.9; Resize icons | `public/favicon*`, `public/apple-touch-icon*`, `public/og-image*` | □ Yes □ No | | Restore original icon files |
| F2.1; LCP eager loading | `src/components/*.tsx` | □ Yes □ No | | Change `loading="eager"` back to `loading="lazy"` |
| F2.2; Preload LCP image | `index.html` or `src/components/*.tsx` | □ Yes □ No | | Remove `<link rel="preload">` or revert `<picture>` |
| F2.3; Code-split JS | `vite.config.ts` | □ Yes □ No | | Remove `manualChunks` config |
| F2.4; Fix forced reflows | `src/**/*.ts` | □ Yes □ No | | Revert geometry caching changes |
| F2.5; Optimise DOM | `src/components/*.tsx` | □ Yes □ No | | Restore removed hidden nodes |
| F3.1; External API to build time | `src/**/*.ts`, config files | □ Yes □ No | | Restore client-side fetch |
| F3.2; CSP headers | `netlify.toml` / `vercel.json` | □ Yes □ No | | Remove or relax CSP directives |
- `references/performance-deep-dive.md`; LCP, CLS, TBT root cause trees
- `references/security-headers.md`; Complete CSP/HSTS/COOP reference
- `references/image-optimization.md`; WebP/AVIF conversion pipelines