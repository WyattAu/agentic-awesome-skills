- Google Workspace (use `google-workspace-attack`; not yet built)
### Method 1: `/api/v1/authn` differential
# Probe single user: DON'T spray, this counts as auth attempt!
#   401 + "errorCode":"E0000004" → invalid credentials (user exists OR doesn't: Okta unifies these)
⚠ Okta has hardened against direct user-existence enum via `/api/v1/authn`; error message is typically uniform "Authentication failed". User enumeration via this endpoint is unreliable in 2024+.
### Method 2: Timing limitations
### Method 3: Sign-in widget JS endpoint
### Method 4: Org-specific identifier probing
### Method 5: OIDC `/v1/authorize` with login_hint
# Initial auth: observe what factors come back
**Critical insight:** the factor list reveals which factors are available; phishing-resistance varies dramatically:
- `webauthn` (FIDO2); phishing-resistant
- `question` (security questions); extremely weak; KBA attacks
- `sms` / `call`; phishing-able (push notification fatigue, SIM swap)
- `push`; phishing-able via MFA fatigue
- `email`; phishing-able if attacker has email read access
- `totp`; phishing-able via AiTM
# Same /api/v1/authn: see authentication flow above
| `200 status=MFA_REQUIRED` | **Password is VALID**; MFA challenge waiting |
| `401 E0000004` | Authentication failed (user doesn't exist OR wrong password; Okta unifies) |
⚠ **OUT OF SCOPE in most red-team engagements** (counts as social engineering / phishing; e.g. phishing was explicitly OOS for authorized-engagement). Document the vector existence but do not execute without explicit sign-off.
# A real test would loop this: DON'T do that without explicit OK
# Iterate known app IDs (find via the org's app list: usually in JS bundles or initial login redirects)
## Okta-specific phishing kits (informational: OOS for non-phishing engagements)
- **EvilProxy**; Okta-aware AiTM kit
- **Modlishka**; generic AiTM
- **Evilginx2**; has Okta phishlets
| `<tenant>.okta.com/api/v1/iam/orgs` returns 401 (not 404) | API IAM endpoints enabled; admin attack surface |
| `customize/sign-in` page reachable anon | Tenant brand customization is public; useful intel |
| `oktapreview.com` subdomain | Preview/sandbox tenant; typically weaker security |
- **`OktaTerrify`** (github.com/silverhack/OktaTerrify); post-compromise Okta device-trust / FastPass enumeration. The only verifiable public Okta-specific offensive tool; no other named, maintained "okta-attacker"/"okta-toolkit" utility is verifiable; build engagement-specific scripts against `/api/v1/*` instead of citing unverified tool names.
- **DO NOT use Entra-style spray pace on Okta**; Okta's anti-automation is tuner-different; rate-limit hits faster
- **DO NOT skip factor enumeration**; knowing the factor list before attempting spray informs the realistic threat model
- **DO NOT assume MFA-fatigue is in scope**; it's social engineering; explicit OK required
- **DO NOT confuse `*.oktapreview.com` with production**; preview is a non-prod tenant, findings have different severity
- `m365-entra-attack`; sibling skill for the M365 case; identical mental model
- `hunt-oauth`; OIDC redirect_uri tampering, state attack, PKCE bypass
- `hunt-saml`; XSW / signature-stripping for per-app SAML SP
- `hunt-mfa-bypass`; push fatigue, OTP brute, replay
- `mid-engagement-ir-detection`; Okta SOC dashboards are sensitive; expect mitigations during testing
Several techniques publicly documented through 2022 (e.g., `/api/v1/authn` differential errors) have been hardened. Don't rely on stale knowledge; confirm enumeration vector freshness on each engagement by:
### 1. Okta + Sitel/Sykes: LAPSUS$ supply-chain breach (Jan 2022, disclosed Mar 2022)
- Year: 2022. Severity: Critical (industry-shaking; reset the trust model for IdP supply chains).
### 3. Scattered Spider / Octo Tempest / UNC3944: Okta tenant social-engineering campaign (2022-2024, ongoing)
- Root cause: Two notification-response action handlers wired to the same backend confirmation path; UX-level Deny did not propagate as a backend rejection.
### 7. Varonis Threat Labs: "CrossTalk" + "Secret Agent" Okta abuse (Jan 2023)
- Flow: (a) CrossTalk: any Okta tenant admin (incl. free-developer tenant) could issue SMS / email templates that delivered to *any* email/phone; sent via legitimate Okta mailer infrastructure (passes SPF/DKIM/DMARC for okta.com). Used to stage cross-tenant phishing that arrives from a trusted sender. (b) Secret Agent: the SSWS token stored on the on-prem Okta AD-agent sync server was decryptable from disk; possessor could register a rogue AD agent that intercepted DelAuth plaintext credentials for the entire org.
- Flow: Class of attack; stealer malware (Lumma, RedLine, Raccoon, StealC) on a corp endpoint exfiltrates `sid` cookie from `<tenant>.okta.com` and `<tenant>-admin.okta.com`. Without IP-binding or device-binding on the Okta session, the attacker replays the cookie from a residential proxy and obtains the user's full session (incl. admin if the victim was an admin); bypasses MFA entirely (already-MFA-completed session). Underpinned both the Oct 2023 HAR-file incident and most Scattered Spider intrusions.
- Root cause: Okta session cookies (until DPoP / Device Bound Session Cookies / asymmetric session keys are enforced) are bearer tokens; anyone holding the cookie is the user.
- The 2022 LAPSUS$, 2023 HAR-file, and 2023 Scattered Spider campaigns share a pattern: **the attack rarely hits Okta's product code; it hits the trust relationships around Okta** (third-party support, help-desk verification, customer-uploaded artifacts, federated IdPs). Recon should map these trust edges first.
- The 2024 delegated-authentication username advisory concerns bcrypt truncation; CVE-2024-0981 instead identifies an Okta Browser Plugin reflected XSS issue; most disclosed Okta bugs are configuration or operational.
- Push fatigue is not just social engineering; the Okta Verify iOS bug shows the platform itself can silently approve a denied push. That historical fixed bug does not establish a current universal push bypass.
- **`hunt-subdomain`**; Okta tenant naming patterns (`<org>.okta.com`, `<org>.oktapreview.com`, `<org>-admin.okta.com`) frequently include orphan/dev tenants. Chain primitive: Okta tenant discovery via `/.well-known/okta-organization` → enumerate `<org>-dev`, `<org>-uat`, `<org>-test` subdomains → `hunt-subdomain` orphan-tenant identification → claim abandoned tenant → SSO takeover (legitimate `<org>` users redirected through compromised IdP for any app federated to the dev tenant).
- **`m365-entra-attack`**; Okta-as-IdP for M365 is common in hybrid orgs. Chain primitive: `okta-attack` user enumeration + spray succeeds on Okta tenant → Okta is federated to Entra → SAML assertion issued by compromised Okta user → full M365 access without ever touching `login.microsoftonline.com` directly (bypasses Entra Conditional Access in many configurations).
- **`hunt-saml`**; Okta issues SAML assertions to every federated downstream app. Chain primitive: Okta admin or developer credential captured → mint arbitrary SAML assertions in Okta admin → `hunt-saml` XSW or signature manipulation not even needed; legitimately signed assertions for arbitrary impersonation across every federated app (Salesforce, Workday, AWS, GitHub, M365).
- **`hunt-mfa-bypass`**; Okta supports multiple factors with varying enforcement. Chain primitive: Okta password sprayed → MFA challenge → `hunt-mfa-bypass` factor-downgrade (push-fatigue, SMS fallback, voice fallback, security-question fallback) → bypass to authenticated session.
- **`triage-validation`**; Okta findings can be high-impact but need the 7-Question Gate run on whether the captured artifact (token, code, factor) actually grants meaningful access. Chain primitive: validated Okta primitive → `triage-validation` to confirm access plane → `redteam-report-template` with explicit federated-app blast-radius.