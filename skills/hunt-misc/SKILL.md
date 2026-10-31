MISC vulnerabilities span access control failures, information disclosure, session/auth logic bugs, and misconfiguration; the categories that consistently produce the highest payouts because they map directly to business impact: data exposure, account takeover, privilege escalation, and infrastructure compromise.
- **SaaS platforms with role hierarchies** (Shopify, GitHub, GitLab); any boundary between owner/admin/staff/guest is a privilege escalation surface
- **Identity/auth flows**; invitation links, password reset, SAML SSO, OAuth token scopes
- **Multi-tenant systems**; one tenant touching another tenant's data
- **Internal APIs**; LFS endpoints, pre-receive hooks, internal GraphQL/REST that assume caller is trusted
- **Domain/DNS management features**; transfer controls, subdomain delegation
- **Token/credential management**; PAT scopes, deploy keys, API tokens stored in config fields
X-Request-Id (pitchfork/Rack; check for header injection)
1. **Map all role/permission boundaries**; enumerate every role level (owner → admin → staff → guest → removed) and document what each role *should* see
   **Marker Discipline:** when probing role boundaries by injecting unique tokens / identifiers into per-role test data, markers should be unique per bounded test, and safe for the receiving parser; no universal eight-character/random-word rule establishes proof. Bad markers: `test`, `marker`, `attacker`, `evil`, `admin`, `AAAA`. Good markers: `cpmark987abc`, `x4hd2k9pq`. Before claiming any reflection, search the baseline (no-marker) response for the marker; if it appears naturally, change your marker.
2. **Test invitation flows end-to-end**; accept invitations without completing verification steps; modify invitation tokens; test whether accepting an invitation as a different user grants access
3. **Test post-removal access**; add a user to a resource, remove them, then test if their session/token still grants access (especially after company/org removal)
4. **Fuzz token scope enforcement**; create PATs/tokens with minimal or no scopes, then call API endpoints that *should* require elevated scopes
5. **Test cross-tenant resource access**; as Tenant A, attempt to read/write Tenant B's resources by manipulating IDs, paths, or headers
6. **Probe internal/undocumented API endpoints**; look for LFS endpoints, internal GraphQL operations, pre-receive hook environments, webhook delivery logs
7. **Check SAML/SSO logic**; test signature verification bypass by stripping signatures, modifying NameID, replaying assertions, or manipulating XML namespace
8. **Audit configuration fields for SSRF/token exfiltration**; any URL field in admin settings (Sentry DSN, webhook URL, proxy URL) is a potential SSRF or credential leak
9. **Test password reset and email verification flows**; skip email verification steps; test whether reset tokens are scoped to a single user; test token reuse
10. **Check HTTP header injection points**; any user-controlled input passed into response headers via Ruby/Rack middleware; test CRLF sequences
11. **Verify DNS/subdomain hygiene**; enumerate subdomains, check for dangling CNAME records, verify SPF/DMARC/DKIM records
12. **Test package registry proxy configurations**; look for dependency confusion via forwarded requests to public registries (PyPI, npm, RubyGems)
# Test in pitchfork/Rack apps: inject via query param reflected in Location header
1. **Soft deletes without permission invalidation**; removing a user from an org marks them as removed but doesn't revoke active sessions or cached permission checks; subsequent API calls still pass old auth context
2. **Invitation acceptance without verification gate**; developers implement invitation flow optimistically (assume user who received email is legitimate) and skip re-verification when token is consumed by a different session
3. **Token scope checked at issuance, not at use**; PAT/OAuth scopes validated when token is created but individual API endpoint handlers don't re-check scope, trusting middleware that may have a gap
4. **Role-based access control checked at UI layer only**; frontend hides buttons for restricted roles but backend API endpoints don't enforce the same restriction; direct API calls bypass UI gating
5. **SAML XML parsing quirks**; signature covers only part of the document; XML canonicalization differences allow unsigned content to pass verification; namespace prefix attacks
6. **Config/URL fields trusted as internal**; integration URL fields (Sentry, webhooks) assumed to be set only by trusted admins; maintainer-level roles can modify them to exfiltrate tokens
7. **Ruby header injection via string interpolation**; developer builds HTTP response headers by string concatenation without sanitizing newlines; Rack 3 behavioral changes exposed previously-hidden bugs
8. **Proxy/mirror configs forwarding all requests upstream**; package registries configured to fall back to public registries without restricting which packages are internal-only, enabling dependency confusion
9. **Pre-receive hook environments exposing privileged context**; hook scripts run with access to internal environment variables, git internals, or SSH keys that shouldn't be user-accessible
10. **Multi-device session design conflated with session fixation**; engineers implement "remember me across devices" by issuing non-expiring tokens, treating it as a feature while creating persistent access risk
11. **Server-policy responses mistaken for state-based oracles**; when many different path types return the SAME response shape, suspect a server-side blocklist/policy filter, NOT a real file-existence / user-existence / resource-existence oracle. Engineers add blanket filters (e.g., "block any path ending in `.config`/`.ashx`/`.asmx`/`.svc`") that return the same error regardless of whether the underlying file exists. Don't infer "file exists" from "blocked"; verify with an independent scoped evidence; timing alone and scaling probes do not establish existence. Lesson: SharePoint's `download.aspx?SourceUrl=` returned `"blocked from this Web site by the server administrators"` for `.ashx`/`.asmx`/`.svc`/`.config` extensions regardless of whether the underlying file existed; looked like a file-existence oracle, was actually the extension blocklist. Treating it as the former produced a list of "discovered custom customer-branded endpoints" that didn't actually exist.
| ReDoS regex hardening | Try different anchor points; if `#` is patched, try `?`, `%`, or other RFC-special characters that trigger backtracking |
**Scenario A; Privilege Escalation via Unverified Partner Invitation (Shopify-class)**
**Scenario B; Cross-Tenant Data Persistence After Removal (Shopify/GitLab-class)**
**Scenario C; SAML Signature Bypass for Account Takeover (GitHub Enterprise-class)**
A checklist tells you what to look for one bug at a time. Senior work composes primitives into multi-step engagements that reach impact. Each chain below is built from existing primitives in this skill's methodology and root causes; paired with a secondary primitive from a neighbouring skill. The pattern across them all: **the secondary primitive is what produces impact**. A demonstrated access-control failure can be independently reportable; chains and severity depend on actual impact and program rules.
### Chain 1: Soft-Delete + Post-Removal Session Persistence → Cross-Tenant PII
- **Impact:** API calls keep succeeding for days or weeks post-removal; cached permission checks pass the old auth context. GDPR/CCPA breach exposure; potential extortion leverage; competitive intelligence theft.
### Chain 2: Invitation Token Reuse + Skipped Verification → Privileged Role Injection
- **C.** Server grants the invited role without re-verifying that the consuming session matches the invited email; new identity inherits the invitation's scope.
### Chain 3: CRLF in Ruby Header + Cache Poisoning → Mass Stored XSS at CDN Scale
- **A.** Identify CRLF injection in a Ruby `Net::HTTP` / Rack response header; user-controlled value flows into `Location:` or a custom `X-*` header (root cause #7: Ruby header injection via string interpolation).
### Chain 4: SAML XSW + Parser Differential → Admin Claim Injection → SSO ATO
### Chain 5: Token-Scope Check at Issuance, Not at Use → Cross-Tenant Write via Low-Scope PAT
- **Impact:** PAT scope model is functionally broken; every read-only token is write-equivalent on the affected endpoints. Mass-exploitable across the userbase by anyone with a leaked PAT.
### Chain 6: Subdomain Takeover at OAuth `redirect_uri` Allowlist → Auth-Code Theft → ATO
- **B.** Find a takeover-able subdomain (`legacy.target.com` CNAME'd to deleted Vercel project / Heroku app / S3 bucket; `hunt-subdomain` step 1).
- `hunt-auth-bypass`; Chains 2, 4, 5
- `hunt-cache-poison`; Chain 3
- `hunt-saml`; Chain 4
- `hunt-subdomain`; Chain 6
- `hunt-ato`; Chains 1, 2, 5, 6 (all terminal-impact paths)
- **`hunt-saml`**; SAML signature wrapping (XSW1–XSW8) is the canonical "misc auth" critical. Chain primitive: SAML XSW + `hunt-saml` AttributeStatement injection → NameID swap → ATO of victim admin via SSO with no password.
- **`hunt-business-logic`**; Misc role/permission desync bugs overlap with business-logic state-machine flaws. Chain primitive: business logic (invitation-before-verify) + role assignment without identity confirmation → tenant takeover.
- **`hunt-auth-bypass`**; Session-revocation gaps and stale-token issues are pure auth-bypass primitives. Chain primitive: removed user retains session token → `hunt-auth-bypass` → post-termination data exfil and persistent access.
- **`hunt-ato`**; Most misc auth bugs end at account takeover. Chain primitive: signature-stripping / NameID injection + `hunt-ato` Path 6 (JWT/SAML manipulation) → admin ATO across enterprise.
- **`security-arsenal`**; Load the SAML Raider payload pack, the session-revocation probe checklist, and the Always-Rejected list (rate-limiting on auth, theoretical issues, user enumeration without sensitive PII).
- **`triage-validation`**; Apply the 7-Question Gate plus the Body-Diff Rule: misc bugs are the highest-N/A category; a state desync claim needs a concrete cross-tenant read or admin-action PoC, not just "the API let me call it".
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.