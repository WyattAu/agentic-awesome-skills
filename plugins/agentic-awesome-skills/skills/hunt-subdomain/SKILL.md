Subdomain takeover is high-value because it allows an attacker to serve content from a **trusted, company-owned domain**; creating a trusted-host content risk whose impact depends on application and browser boundaries.
- Preview/staging/dev subdomains (`new.`, `preview.`, `course.`, `delivery.`, `addons-preview.`); abandoned after feature launches
- Subdomains used for OAuth redirect URIs or SSO endpoints; these pay highest
   - `dig CNAME subdomain.target.com`; confirm CNAME exists
   - `dig A <cname-target>`; confirm NXDOMAIN or no resolution
   - `curl -sk https://subdomain.target.com`; check for provider error string
5. **Confirm claimability**; attempt to register the resource:
6. **Claim the resource** (only enough to prove control; do NOT serve malicious content):
# If pointing to *.gitlab.io: visit the gitlab.io URL directly
1. **Service offboarding without DNS cleanup**; Developer removes a Heroku app, UserVoice account, or WordPress site but never deletes the CNAME record. DNS lives forever; service does not.
2. **Staging/preview infrastructure abandoned post-launch**; `course.`, `new.`, `preview.`, `beta.` subdomains provisioned for a product launch, pointed at a third-party, then forgotten when the campaign ends.
3. **Subdomain provisioned by a third-party team**; Marketing sets up a UserVoice or Zendesk subdomain via IT, product sunset kills it, but DNS is owned by engineering who doesn't know.
4. **CDN misconfiguration without origin validation**; Fastly and similar CDNs historically allowed any domain to "claim" a backend hostname by creating a service pointing to it. Unregistered origin hostnames become claimable.
5. **GitHub/GitLab Pages namespace not reserved**; Organization renames, user accounts deleted, or repos made private/deleted while the Pages CNAME still points to the old namespace.
6. **Wildcard DNS entries**; `*.target.com` pointing to a cloud provider means *any* unclaimed subdomain potentially resolves to claimable infrastructure.
7. **Acquired/divested company DNS not cleaned**; Post-acquisition, former brand subdomains (like `oberlo.com` under Shopify) retain CNAMEs to services that are no longer paid for.
- Bypass: Use alternative error strings; providers change their 404 pages. Maintain an up-to-date fingerprint list. Some providers show *different* errors on HTTP vs HTTPS. Test both.
- Bypass: Some providers (GitHub Pages, Netlify) auto-provision TLS for claimed domains. Others don't; show HTTP takeover and note TLS would be resolved by provider on claim.
   Can you register the unclaimed resource (GitHub repo, S3 bucket, Heroku app, UserVoice account) and serve arbitrary content; including phishing pages, credential harvesters, or malicious scripts; under the target's trusted domain name?
**Scenario A; Trusted Brand Phishing via Abandoned SaaS (Snapchat/UserVoice)**
An attacker finds `feedback.snapchat.com` CNAME pointing to a UserVoice subdomain. The UserVoice account was cancelled but the DNS record remained. The attacker registers the matching UserVoice subdomain for free, gaining control of `feedback.snapchat.com`. Any user navigating to that URL; perhaps from old bookmarks or Google results; sees attacker-controlled content on a Snapchat-branded domain. Since the domain is trusted by browsers, phishing campaigns sent from this subdomain bypass email security filters that check domain reputation.
**Scenario B; CDN Origin Takeover Enabling Same-Origin Attacks (Mozilla/Fastly)**
**Scenario C; Staging Subdomain Abandoned Post-Product Migration (Rails/GitHub Pages)**
`new.rubyonrails.org` was pointed at a GitHub Pages deployment for a website redesign project. After the new site launched and the old GitHub repo was deleted or made private, the DNS CNAME remained. An attacker could fork or create a matching GitHub Pages repository and claim the namespace, serving content under `new.rubyonrails.org`. Because this is the official Ruby on Rails domain, any content served there; including fake download links or malicious gems; carries the full trust of the Rails brand.
The following coordinated-disclosure / writeup cases extend this skill with **modern provider fingerprints** (Vercel/Azure cloudapp/Zendesk era) and explicit ATO-chain examples. Citations #12 and #13 are external writeups (not disclosed HackerOne reports); treat their payout figures as unverified; only #14 has a public H1 report ID.
12. **Microsoft Azure DevOps; Two `cloudapp.azure.com` subdomains + wildcard `*.visualstudio.com` OAuth reply_to → 1-click ATO** ([Binary Security writeup](https://www.binarysecurity.no/posts/2022/11/azure-devops-takeover))
    - Subclass: Azure `cloudapp.azure.com` regional-pool dangling CNAME; chained to ATO
    - ATO chain: **YES**; `app.vssps.visualstudio.com/_signin?reply_to=https://feedsprodwcus0dr.feeds.visualstudio.com/` whitelisted any `*.visualstudio.com`. Attacker claimed the dangling Azure VM hostnames, then crafted sign-in URLs that returned JWT + FedAuth tokens to attacker-controlled endpoints
    - Year: reported Feb 2021, disclosed Nov 2022; MSRC explicitly out-of-scope (relied on subdomain takeover), $0
13. **Anonymous H1; `admin-support.xyz.com` → unclaimed Zendesk → email interception → ATO** ([Writeup by 0xprial](https://0xprial.com/the-art-of-zendesk-hijacking/))
    - ATO chain: **YES**; researcher configured email forwarding on the hijacked Zendesk instance, intercepted `support@xyz.com` tickets containing payment info + password-reset emails, then triggered password resets on customer accounts that delivered reset links into the attacker's Zendesk inbox
    - Year: 2023; payout per the 0xprial writeup (blog source; no public HackerOne report ID, treat as unverified)
14. **Vercel deleted-project takeover**; a dangling `cname.vercel-dns.com` CNAME pointing to a removed Vercel project can be re-claimed by deploying a new project under that name.
    - Impact: crypto-DEX phishing; `proxies.` subdomain trusted for RPC proxy routing; attacker could serve malicious wallet-drain JS under a "trusted" subdomain
    - Claim flow: subdomain returns Vercel 404 `DEPLOYMENT_NOT_FOUND` → create free Vercel project → Settings → Domains → add `proxies.sifchain.finance`; Vercel verifies the existing CNAME and auto-issues TLS without out-of-band ownership proof
    - Year: 2022; Sifchain treated as Critical (web3 phishing vector)
Subdomain takeover by itself is Low-Medium / Informational on most mature programs; defacement of a non-business-critical subdomain is unsexy. The chain payout is 10-100x the standalone. **Every takeover should be evaluated against the five chains below before submission.** Severity depends on demonstrated impact and program policy, not the number of proposed chains.
### Chain 1: Takeover + OAuth `redirect_uri` Whitelist → Auth-Code Theft → 1-Click ATO
- **B.** Find a takeover-able subdomain in that allowlist. Claim it via the provider's onboarding (Vercel project, Azure cloudapp regional pool, S3 bucket, Heroku app, Zendesk, Shopify storefront; see the Disclosed Report Citations above).
- **Real shape:** Microsoft Azure DevOps `cloudapp.azure.com` + wildcard `*.visualstudio.com` reply_to chain (Binary Security, Nov 2022; Disclosed Report Citation #12). Multiple H1 disclosures on SaaS programs with permissive OAuth allowlists.
### Chain 2: Takeover at Sibling Subdomain + Cookie-Domain Wildcard → Session Fixation on Parent App
- **A.** Inspect cookies set by the main app (`app.target.com`). If `Set-Cookie` has `Domain=.target.com` (parent-scoped) instead of host-only, cookies bleed to every sibling subdomain; including taken-over ones.
- **Impact:** ATO without OAuth or password reset; pure cookie-domain bleed. Especially effective when the parent app uses a non-`__Host-` prefixed session cookie.
### Chain 3: Takeover + CSP `script-src` Includes the Taken-Over Host → Persistent Stored XSS on Main App
- **C.** Host attacker-controlled JavaScript at the takeover host. An actual script reference on the main app may fetch `<script src="//taken-over-host/x.js">` because the host is on the CSP allowlist. JS executes with main-app origin; full session access, can call any same-origin API.
- **Real shape:** Sifchain `proxies.sifchain.finance` Vercel takeover; Disclosed Report Citation #14 (web3 phishing); pattern documented in multiple H1 disclosures 2020-2024.
### Chain 4: Takeover + CORS `Access-Control-Allow-Origin` Regex Match → Credentialed Cross-Origin API Read
- **Impact:** Mass cross-tenant API read with credentials; sessions, PATs, account data, billing records; all reachable from a single attacker page.
### Chain 5: Takeover at Email DNS (DKIM / SPF / MX) → Email Spoofing → Phishing Trusted by Parent Brand
- **A.** Enumerate the target's email DNS; DKIM selectors (`selector1._domainkey.target.com`), SPF includes (`include:_spf.takeover-candidate.com`), MX records (`mx.target.com → defunct-provider.example`).
1. **OAuth `redirect_uri` allowlist**; does the taken-over host appear? → Chain 1, Critical.
2. **Parent-domain cookies**; does the main app set `Domain=.target.com`? → Chain 2, High.
3. **CSP `script-src`**; does the taken-over host appear in the allowlist? → Chain 3, Critical.
4. **CORS allowlist**; does any regex match the taken-over host? → Chain 4, High.
5. **Email DNS (DKIM selector / SPF include)**; does the taken-over host appear? → Chain 5, High.
If none apply, file at Low/Informational. **Do not file at Critical without demonstrating one of these chains**; triagers downgrade fast otherwise.
- `hunt-oauth`; Chain 1 (`redirect_uri` bypass class)
- `hunt-auth-bypass` Duende BFF Attack Class 2; Chain 2 (cookie scoping)
- `hunt-xss` Chain 4; Chain 3 (CSP bypass via trusted-origin JS)
- `hunt-api-misconfig` CORS section; Chain 4
- `offensive-osint` email-security section; Chain 5
- **`hunt-cloud-misconfig`**; Most stale CNAMEs point at deleted cloud assets (S3, CloudFront, Heroku). Chain primitive: Cloud misconfig (S3 deleted) + `hunt-subdomain` → unclaimed CNAME points to bucket → claim bucket name → full subdomain control.
- **`hunt-oauth`**; A takeover on an OAuth `redirect_uri` host = persistent ATO across the entire SSO surface. Chain primitive: Subdomain takeover at `auth.target.com` + OAuth redirect_uri allowlist → auth code theft → ATO every user that re-authenticates.
- **`hunt-api-misconfig`**; CORS regexes routinely allowlist a takeoverable subdomain. Chain primitive: Subdomain takeover + CORS `*.target.com` with credentials → credentialed cross-origin API read → mass IDOR.
- **`hunt-xss`**; A claimed subdomain is cross-origin to siblings; cookie scope is separate. Chain primitive: Subdomain takeover at `feedback.target.com` + cookie scope `.target.com` → JS hosted on takeover host reads main-app cookies → session hijack.
- **`security-arsenal`**; Load the 27+ Subdomain Takeover Fingerprint Table (NoSuchBucket, "no such app", GitHub Pages 404 strings, Heroku, Shopify, Fastly) and the `subzy`/`subjack` automation patterns.
- **`triage-validation`**; Apply the Unique-Marker gate: takeover claim is informational on its own; submit only after publishing a unique HTML marker on the claimed host AND demonstrating a downstream impact (cookie read, OAuth chain, CSP bypass).
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.