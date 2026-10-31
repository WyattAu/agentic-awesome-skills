3. **Use a "simple-request" Content-Type**; `application/x-www-form-urlencoded`, `multipart/form-data`, OR `text/plain` are the three CORS "simple" content-types a cross-origin form can send with no preflight. A JSON endpoint is CSRF-resistant **only if the server rejects those**; if it also accepts a `text/plain` body (common), craft a `text/plain` payload that parses as valid JSON (see the JSON-CSRF-via-text/plain section). Don't skip a JSON endpoint on the assumption that `application/json` alone is protective.
- Omit the token field entirely; some frameworks only validate if the field exists, not if it's absent
- Send an empty value (`_token=`); some validate format, not presence
- Copy a token from another session; some tokens aren't tied to the session
- **Third-party integrations**: Grafana, monitoring dashboards, embedded analytics; often lag on CSRF protections
- **Social platforms**: Twitter/X collections, friend imports, social graph mutations; high-volume, authenticated actions with real user impact
authenticity_token        // Rails; check if reused across page loads
- **Rails apps**: Look for `authenticity_token`; test if it's static per session
- **Django apps**: Check `csrfmiddlewaretoken`; test cross-user/session reuse
- **Grafana instances**: CVE-2022-21703; check version via `/api/health`
1. **Map all state-changing endpoints**; Spider authenticated session, filter for POST/PUT/DELETE/PATCH. Note every form and AJAX call.
2. **Check cookie SameSite attributes**; In DevTools → Application → Cookies. Flag any session cookie without `SameSite=Strict` or `Lax`.
3. **Test token staticness**; Log in twice (different sessions or incognito). Compare `authenticity_token` / `csrfmiddlewaretoken` / `csrf-token` values across:
4. **Test token omission**; Remove the CSRF token field entirely from a POST request. A 200 response alone does not prove CSRF; verify browser delivery and state change.
5. **Test token substitution**; Replace the token with one from a different session. Server accepting it = broken validation.
6. **Test JSON endpoints for form-POST CSRF**; Check if Content-Type is enforced:
7. **Hunt OAuth/SSO RelayState**; Intercept SAML/OIDC flows. Test if `RelayState` is validated for same-origin. Inject external URLs.
8. **Check social linking flows**; Every "connect your X account" feature. These often use redirect-based OAuth where CSRF on the callback can associate an attacker's social account.
9. **Test third-party dashboards on subdomains**; Grafana, Kibana, Prometheus. Check version, apply known CVEs, test default CSRF posture.
10. **Build PoC HTML page**; Host on a different origin, fire the request, confirm cookies are sent and action executes.
1. **Predictable, exposed or improperly bound tokens**; A random per-session token can be legitimate; stability alone is not a vulnerability.
2. **Token not tied to user identity**; Token is valid server-wide or rotates on a schedule, not per-user/session. Mozilla bug: `csrftoken` reusable across users.
3. **Missing token on "secondary" endpoints**; Developers protect login/signup but forget API endpoints, import flows, or webhook handlers.
4. **JSON API assumption of safety**; Belief that `Content-Type: application/json` prevents CSRF. It does via CORS preflight; unless the server also accepts `text/plain` or `application/x-www-form-urlencoded`.
5. **SameSite=None for cross-site embeds**; Developers set `SameSite=None` to support iframe embeds or third-party integrations, inadvertently re-enabling CSRF.
6. **OAuth RelayState not validated**; Developers implement SAML/OIDC but treat `RelayState` as a redirect hint, not a CSRF state parameter requiring cryptographic binding.
7. **Framework misconfiguration**; CSRF middleware excluded for `/api/*` routes in Django/Rails because "API clients don't need it," but browser-based JS clients do.
8. **Third-party software defaults**; Grafana, Kibana, Jenkins shipped with weak or no CSRF protection in older versions; teams don't patch or check.
- Token is static per session; steal via XSS, Referer leakage, or cached page
- Token not validated server-side; just remove it and try
- Token validated by length/format only; submit a fake but correctly-formatted value
**Bypass:** Simple requests (form POST, `text/plain`) don't trigger preflight and can't set custom headers; but some servers only check for header *presence*, not value, and some frameworks accept requests without it.
CSRF is not browser-only. For targets with a mobile app, enumerate exported deeplink / custom-scheme handlers (`scheme://action` from the Android manifest / iOS `Info.plist`) and test whether opening one performs a state-changing action (follow, link, purchase) using the app's ambient session with no CSRF token; deliverable to a victim by a plain link or a QR code. Disclosed: reports/583987, reports/805073.
1. **What can the attacker DO right now?**; The attacker must be able to trigger a specific state-changing action (account linking, email change, data deletion, social association) on behalf of the victim without any interaction beyond visiting a URL or page.
2. **What does the victim LOSE?**; Identify the concrete harm: account access (ATO), data exposure, financial loss, reputation damage. "A CSRF token is missing" is not impact; "attacker can link their Oculus account to victim's Facebook account, gaining full profile access" is impact.
3. **Can it be reproduced in 10 minutes from scratch?**; You must be able to: (a) create attacker and victim accounts, (b) host a static HTML PoC, (c) have victim visit PoC, (d) confirm the action executed in victim's account; with documented prerequisites and reasonable effort; ten minutes is not a validity requirement.
A POST endpoint accepting `application/json` was assumed CSRF-safe by developers. A researcher crafted an HTML form using `enctype="text/plain"` with an input name designed to produce syntactically valid JSON when submitted. The browser sent the request cross-origin without a preflight (no custom headers, `text/plain` is a simple request), cookies were attached, and the server processed the JSON body as legitimate; silently logging attacker-controlled activity data under the victim's account identity.
## Disclosed Report Citations (Backfill +5: 2020-2024)
11. **Argo CD; SameSite=Lax bypass via sibling subdomain + Content-Type abuse (CVE-2024-22424)** ([GHSA-92mw-q256-5vwg](https://github.com/argoproj/argo-cd/security/advisories/GHSA-92mw-q256-5vwg) · [Writeup](https://blog.calif.io/p/argo-cd-csrf))
    - Subclass: SameSite=None/Lax misconfig chain; same parent-domain bypass + JSON CSRF via missing Content-Type enforcement
12. **GitLab; CSRF on `/api/graphql` via GET-converted mutations** ([H1 #1122408](https://hackerone.com/reports/1122408))
    - Year: 2021; **$3,370**
13. **Stripe Dashboard; CSRF middleware disabled by code change** ([H1 #1483327](https://hackerone.com/reports/1483327))
    - Subclass: framework misconfiguration; middleware globally disabled
    - Year: 2022; **$5,000** ($2,500 × 2 researchers)
14. **GitHub Enterprise Server; CSRF bypass via path traversal (CVE-2022-23732)** ([H1 #1497169](https://hackerone.com/reports/1497169))
    - Year: 2022; **$10,000**
15. **HackerOne self; CSRF on social account linking → ATO** ([H1 #1727221](https://hackerone.com/reports/1727221))
    - Payload: `<img src="https://hackerone.com/users/social_accounts/google?code=ATTACKER_CODE&state=PREDICTABLE">`; victim's browser completes attacker-initiated link flow
    - Root cause: token bound to OAuth-link callback was either reused across attempts or not user-bound, so attacker-issued link callbacks were accepted on the victim's session; attacker's Google account becomes a valid login path = ATO
    - Year: 2022; informational scope on H1 self-program, but public PoC
- **`hunt-xss`**; Any XSS on a trusted origin neutralizes CSRF defenses (token, SameSite, Origin check) instantly. Chain primitive: XSS reads the `meta[name=csrf-token]` value and same-origin-fetches `/accounts/email` with attacker payload → one-click ATO via attacker-page postMessage triggering the stored XSS to perform the state change.
- **`hunt-auth-bypass`**; CSRF combined with an auth-bypass primitive lets attacker-side scripts perform state changes that should have required step-up auth. Chain primitive: CSRF on `/settings/password` reaches an endpoint that skips the re-auth check → password change executes without the victim ever entering their current password → ATO.
- **`hunt-oauth`**; OAuth/SAML `state`/`RelayState` is structurally a CSRF token; missing validation here is account-linking CSRF. Chain primitive: attacker initiates OAuth on their account, sends victim the `/callback?code=X&state=` URL → victim's logged-in browser completes the link → attacker's social identity now controls victim's account.
- **`security-arsenal`**; Reach for the CSRF PoC templates (form POST, `enctype=text/plain` JSON, sandboxed-iframe null-origin, base64 multipart bypass) before writing one from scratch; also the WAF-bypass header variants for Origin/Referer checks.
- **`triage-validation`**; Run the Pre-Severity Gate before submitting CSRF on a logout endpoint or any action without state-change consequence; those are the canonical N/A traps. Confirm victim LOSES something concrete (account access, money, data), not just "a request executed."
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.