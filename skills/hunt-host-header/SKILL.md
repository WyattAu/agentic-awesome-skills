# HUNT-HOST-HEADER: Host Header Injection
- **Password-reset poisoning class**; the canonical write-up is Skelet's/Detectify-era
- **Web cache poisoning via unkeyed Host / X-Forwarded-Host**; PortSwigger Research,
- **Routing-based SSRF**; PortSwigger Research, "Cracking the lens" / routing-based SSRF
verified; a missing citation is always better than a wrong one.
- **Password reset poisoning → ATO**; server builds the reset link from the request Host;
- **Web cache poisoning via unkeyed Host**; a CDN/reverse proxy caches a response that reflects
- **Routing-based SSRF**; the front-end uses the *Host header itself* to pick the upstream;
- **Path-override SSRF / ACL bypass**; IIS/ASP.NET/Spring honour `X-Original-URL` /
  ACL thought it blocked. (Different layer from routing SSRF; see Phase 3.)
- **OAuth/OIDC poisoning**; Host drives `redirect_uri` or the OIDC `issuer` / discovery doc →
### Phase 1: Password Reset Poisoning
have proof; see Validation.
### Phase 2: Web Cache Poisoning via Host / X-Forwarded-Host
`Location`, or canonical/og:url) **and** that response must be **cached and reusable by another permitted request on the same verified test key**. No Collaborator callback is expected from the cache test itself; only later, if a
  `Vary`, or the CDN includes Host in the key) is **not** poisoning; confirm 2c returns the
### Phase 3: SSRF via Host Header: TWO DISTINCT MECHANISMS (do not conflate)
**(3A) Routing-based SSRF; the Host header selects the upstream.** The path goes on the
here; the EC2 IMDS ignores it.
# proxy's outbound DNS/HTTP lookup: that proves the front-end resolves the attacker host.
**(3B) Path-override SSRF / ACL bypass; `X-Original-URL` / `X-Rewrite-URL`.** This is an
### Phase 4: OAuth / OIDC / SAML Poisoning
Collaborator); a reflected string alone is not ATO.
### Phase 5: Header Fuzzing (Param Miner)
| Reset link host = attacker (own test acct) | Collaborator-host injection → capture token on click | Critical; ATO any user |
| X-Forwarded-Host reflected in absolute URL + cacheable, unkeyed | Poison key → clean fetch returns payload → load XSS/redirect | High; mass cache poisoning |
| Front-end routes by Host | `Host: 169.254.169.254` path-on-request-line → creds | High/Critical; SSRF → cloud creds |
| `X-Original-URL` overrides path | Reach `/admin` blocked at edge | High; ACL bypass / SSRF |
| OAuth redirect_uri/issuer built from Host | Re-point flow → capture code/token on Collaborator | Critical; ATO via code theft |
- Some mailers rewrite links to a fixed `SITE_URL` regardless of Host; reflected header in the
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.