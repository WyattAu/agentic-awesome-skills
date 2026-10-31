2. **Reverse proxy in front?** Heuristic from response headers / CNAME chain (when collected). A non-Vercel CDN over Vercel ISR is usually a "dumb pipe"; wasted spend.
If you find yourself wanting to grep the whole codebase, stop and re-read the current candidate's `question` field. If the question doesn't constrain the search, the candidate is malformed; log it as `gated` and skip. Do NOT compensate with a wider search.
**Default rule:** scanner findings on `COLD-PATH` or `NO-ROUTE-MAPPING` files are dropped. They become recs only if the pattern is *traffic-independent*: build configuration, middleware matcher, source maps in production, raw script tags, React Compiler config. These don't care about traffic; they affect every request equally or affect the build itself.
## Rule 4: Doc-grounded, version-aware recommendations: no hallucinations
- **URLs**; Vercel docs, Next.js docs, SvelteKit docs, etc. Each declares `applicableFrameworks` (e.g., `["next@>=15.0.0"]`).
- **Cross-skill rule references**; by name only (`vercel-react-best-practices:async-parallel`). The agent's host resolves these.
- `missing-citation`; drops recs with empty `citations[]`.
- `unknown-citation`; strips URLs not in the library, marks `needsReview=true`.
- `version-mismatch`; strips URLs whose `applicableFrameworks` doesn't match the project's framework@version (parsed from `package.json`).
Every performance claim cites the actual observability datum from `signals.json`; e.g., `functionRoutes[/api/products].p95Ms=850`. Estimated improvements are framed as ranges grounded in the observed baseline: `"Reduce /api/products 95th percentile duration from 850ms toward ~250-400ms based on similar cached routes."` Never an unanchored claim.
The `$-strip` sanitizer enforces this at output time; any `$N` literal in customer-facing fields is stripped.
- "Save $340/mo by doing X"; invented precision.
- **Build-time issues without runtime impact.** Slow builds, build-cache misses, monorepo build fan-out; these only enter scope when they show up as Build Minutes billing pressure (then they go through the `build-minutes-fanout` gate). A 6-minute build that completes successfully and ships a small artifact is not a target.
- **Security advisories and credential rotation.** RCE in `next-mdx-remote`, leaked env vars, OIDC vs explicit-key auth hygiene; refer to a security skill, not this one. Exception: when a security setting is also a documented cost lever (BotID = bot traffic = edge cost), it enters via the `platform_bot_protection` gate.