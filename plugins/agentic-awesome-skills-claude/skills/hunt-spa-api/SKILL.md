The core insight: **a SPA is a client to a backend API, and it ships the full map of that API; hosts, routes, sometimes keys; to anyone who views source.** The login page being protected says nothing about whether the API behind it checks tokens. Auth is frequently enforced on the *gateway/login* and missing on a *route group* of the API.
DO NOT skip this because "the app needs login"; that's exactly when this pays off.
# Backend API hosts (incl. dev/beta/staging variants: often weaker auth)
# Route literals: minified bundles store routes as STRING segments, not full URLs.
# Secrets (validate before trusting: most AIza keys are Maps/analytics, not Auth)
**Note:** minifiers store routes as concatenated string segments (e.g. `"account/payment/list"`), NOT full `/api/v2/...` URLs; so a naive `/api/v*` grep returns nothing. Grep for the **resource-word route strings** and prepend the base yourself.
**Lazy-loaded (async) chunks; don't stop at HTML-referenced bundles.** CRA/webpack SPAs reference only runtime+main+vendor in index.html; numbered async route chunks are named inside main.js's chunk map and loaded at runtime, so grepping only `/static/js/*.js` from the shell truncates route coverage to eager chunks.
### 3. Establish a CONTROL: find an endpoint that IS gated
This is your differential. A sibling API (e.g. a second API host, or a different route group on the same host) is the ideal control; same stack, so a different response = real authz gap, not a quirk.
- **Mandatory fields named like `is_admin` / `is_internal` / `requested_by` / `role_id` / `account_type`** → **authorization derived from client-supplied parameters**; set the privilege flag and you self-elevate. Critical-class.
- IDs returned by one endpoint (`account_id`, `order_id`, `deal_id`) are the keys the *other* endpoints consume; they prove the whole router is reachable, not just one route.
- Test `dev-`/`beta-`/`staging-` API variants; they frequently have weaker/disabled auth.
- **STOP at minimum-necessary proof.** A handful of records (or a `totalCount`) confirms the missing check. Do NOT enumerate the table; see `redteam-mindset` data-minimization boundary. The finding is the absent auth, not the data volume.
- **"The app requires login, so the API must be protected."** No; test the API directly, unauthenticated. The whole point.
- **"Minified bundle, can't read it."** You don't need to read it; grep it for hosts/routes/secrets. 5 minutes.
- **"`/api/v1/foo` returned 404, so no API here."** Wrong base or wrong method. Try `/api/`, `/api/v2/`, POST not GET, and the exact route strings from the bundle (Express's 404 echoes the path; use it to calibrate).
- **"AIza key found → critical secret."** Validate first; most are Maps/analytics keys (`CONFIGURATION_NOT_FOUND` on identitytoolkit = not Auth-enabled). Don't over-claim.
- **Account-creation / write endpoints as "proof".** Read endpoints prove the auth gap without creating state. Never POST a `create`/`signup`/`upload` to "demonstrate"; that's a destructive write needing explicit per-action authorization.
- **`hunt-api-misconfig`**; once the API is mapped, run the broader misconfig matrix (method tampering, mass assignment, JWT alg confusion) per route.
- **`hunt-idor`**; the `account_id`/`order_id` pivots feed straight into IDOR testing across discovered routes.
- **`hunt-source-leak`**; sourcemaps (`*.js.map`) reconstruct original source for deeper route/secret extraction; same harvesting muscle.
- **`hunt-nextjs`**; for Next.js targets, layer the middleware-bypass (`x-middleware-subrequest`) and `/_next/data` route tests on top of this.
- **`redteam-mindset`**; the data-minimization boundary governs step 5: prove the missing check, don't exfiltrate the table.
- **`recon-scope-triage`**; verify the API host actually belongs to the target before testing (don't pop a same-named third party's API).