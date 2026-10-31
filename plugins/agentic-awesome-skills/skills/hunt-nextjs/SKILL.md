# HUNT-NEXTJS: Next.js / SSR Framework Vulnerabilities
- **Server Actions auth bypass**; Server Actions enforce auth client-side only → call action ID directly → unauthorized data mutation or exfil
- **Middleware bypass via `/_next/static/`**; middleware skips static asset paths → protected routes accessible via `/_next/data/` IDOR
- **`/_next/image` SSRF**; Image optimizer fetches attacker-controlled URL → internal network scan or cloud metadata
- **ISR stale cache poisoning**; inject malicious content into a cached page that gets served to all users
- **RSC payload leakage**; React Server Component flight data contains server-side props not meant for client
/_next/image?url=&w=&q=          Image optimizer; SSRF candidate
/_next/data/BUILD_ID/*.json      Prerendered page data; IDOR candidate
/_next/static/chunks/            JS bundles; source map candidate
/api/                            API routes; standard hunt surface
## Phase 1: Fingerprint & Version Detection
## Phase 2: Server Actions Abuse
## Phase 3: Middleware Auth Bypass
# Test via /_next/data/ (SSG/ISR JSON): middleware may not apply
Next skips middleware execution entirely when this header asserts an internal subrequest; bypassing auth/redirect middleware on gated routes.
## Phase 4: Image Optimization SSRF (`/_next/image`)
# Basic SSRF test: internal metadata
# OOB detection: use a UNIQUE per-test subdomain so callbacks can't be confused
`next.config.js`. A non-whitelisted `url` returns **400 by default**; that is
> through < 14.1.1, fixed in 14.1.1) is a **Server Actions** SSRF; a relative
> redirect that trusts the `Host` header; NOT a `/_next/image` bug, and it does
## Phase 5: `/_next/data/` IDOR & Data Leakage
## Phase 6: ISR Cache Poisoning
#    Body-diff clean-vs-poisoned and check x-nextjs-cache / age headers: a reflected
## Phase 7: Debug & Stack Frame Endpoints
build (`next build && next start`) does not register these routes; a 404 here
## Phase 8: Environment Variable Leakage
# NEXT_PUBLIC_* vars are baked into JS bundles: grep for secrets
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.