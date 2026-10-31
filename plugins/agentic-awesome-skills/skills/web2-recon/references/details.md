- Out-of-scope list (read carefully; common trap)
The projectdiscovery Go binaries (`dnsx`, `httpx`, `naabu`) occasionally `SIGSEGV` on macOS arm64 due to a cgo / system-resolver interaction. The crash signature is identical regardless of install method; both `brew install` and `go install github.com/projectdiscovery/<tool>@latest` produce binaries that segfault at the same address. Smoke-test once before relying on them in a real engagement:
API spec endpoints are the single highest-leverage recon target on any modern .NET / Node / Python / Java backend. The spec discloses every endpoint, HTTP methods, parameter names + types + formats, models, validation rules; a complete attack-map in JSON. Default routes are commonly left enabled in production. **Add this wordlist to the directory-fuzzing phase** (after the standard `common.txt` pass).
- **Spec → mass IDOR/BOLA**; `jq '.paths | keys' swagger.json` becomes the input list for `Autorize`/`ffuf` per-user testing.
- **Spec → mass-assignment payload construction**; `components.schemas.UserUpdateDto` enumerates `isAdmin`, `emailVerified`, `tenantId`, `role`.
- **Spec → hidden endpoint discovery**; `/internal/*`, `/debug/*`, `/v0/*`, `/legacy/*` routes documented but never auth-gated.
- **Spec → injection-class seeding**; every parameter's type + format + enum + max-length means payloads pass validation before reaching the sink. Especially valuable against ASP.NET Core where the model binder rejects malformed input before any controller logic.
- `kiterunner`; natively ingests OpenAPI spec, generates requests against the API.
- `sj` (Swagger Jacker); purpose-built for Swagger spec exploitation.
- `apidetector` (brinhosa); Swagger-UI mass scanner.
- `XSSwagger` (vavkamil); detects vulnerable Swagger UI versions (CVE-2018-25031 family).
- `nuclei -t http/exposures/apis/`; built-in templates for default spec paths.
- **`offensive-osint`**; When recon needs concrete probes / wordlists / regexes beyond the basic pipeline. Workflow primitive: this skill produces the URL set; `offensive-osint` provides the secret regexes, GraphQL/Swagger paths, and identity-fabric probes you apply to that URL set.
- **`osint-methodology`**; When you need a severity rubric for what you discovered. Workflow primitive: after recon outputs `subdomains.txt` / `live-hosts.txt` / `urls.txt`, score each asset against `osint-methodology`'s findings rubric to decide what gets a finding versus what stays in the asset graph.
- **`hunt-subdomain`**; When recon surfaces stale CNAMEs / dangling DNS. Workflow primitive: any subdomain in `subdomains.txt` whose CNAME points to S3 / GitHub Pages / Heroku / Shopify / Azure should auto-route to `hunt-subdomain` for takeover validation.
- **`security-arsenal`**; When the URL set is classified by `gf` and ready for active testing. Workflow primitive: `gf xss/ssrf/sqli/idor` output names become payload-class queries against `security-arsenal`'s payload library.
- **`bb-methodology`**; When recon completes and Phase 1 transitions to Phase 2 (Mapping). Workflow primitive: hand the live host + URL set back to `bb-methodology` Phase 2 for endpoint mapping and Phase 3 vulnerability discovery routing.
Internal-looking subdomains often expose more surface than the marketing site; `partner.target.com` and `vendor-portal.target.com` frequently have weaker auth than the main app because they're scoped for "trusted" external users. Always send a probe to the long-tail wordlist after the standard subfinder run completes.
- `Server:`; apache / nginx / cloudflare / kestrel (= .NET Core) / openresty / envoy
- `X-Powered-By:`; PHP version, ASP.NET version, Express.js
- `X-Drupal-Cache`, `X-Generator: Drupal 9`; Drupal
- `X-Generator: WordPress`; WordPress
- `Via:`; CDN chain (1.1 varnish, 1.1 cloudfront)
- `Set-Cookie:` names; `JSESSIONID` (Java), `PHPSESSID` (PHP), `ASP.NET_SessionId` (.NET), `connect.sid` (Express), `laravel_session` (Laravel)
The first 10s of recon should yield a stack guess; the rest is targeting. If your fingerprint contradicts itself (Server says nginx, Set-Cookie says ASP.NET) you've found a reverse proxy front-end; note the origin app for later smuggling/cache attacks.
- **"Error - 404"** with `Server: CloudFront` = distribution exists, origin returned 404; NOT a takeover.
Phase 2C verified both patterns live. Always check the EXACT response body string before filing a takeover finding; the takeover-scanner tools (subzy, subjack) match on multiple fingerprints and frequently false-positive on the "still owned, just empty" case.