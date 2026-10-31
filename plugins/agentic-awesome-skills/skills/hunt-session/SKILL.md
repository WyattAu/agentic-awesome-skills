**Missing HttpOnly on cookies may be detected by separately configured tooling; focus your active testing on lifecycle invalidation (higher impact).**
**Pattern 1; Session survives logout (most common high-value finding):**
**Pattern 2; Session not regenerated on login (session fixation):**
**Pattern 3; Session survives password change:**
**Content-type:** Login and session endpoints vary; use `application/json` for REST APIs, `application/x-www-form-urlencoded` for traditional web forms. Try both if the first returns an unexpected response.
# HUNT-SESSION: Session Management
- **Session fixation**; server accepts a session ID set by the client and does NOT regenerate it on login → attacker pre-plants an ID, victim authenticates, attacker rides the now-authenticated session → persistent ATO.
- **No invalidation on logout**; old token still works after `/logout` → theft window never closes.
- **No invalidation on password / email change**; a stolen session survives the victim's "I think I was hacked, let me reset" → persistent ATO. Impact and bounty eligibility depend on the program and demonstrated threat model.
- **Refresh-token reuse without rotation-detection**; a leaked refresh token may mint access tokens within its enforced lifetime; no reuse-detection means the legitimate user's later refresh does NOT revoke the attacker's branch.
- **Predictable / low-entropy session ID**; sequential, timestamp- or userId-derived IDs → brute-force or compute other users' sessions.
- **JWT-as-session with no `exp` / no revocation list**; inspect server-enforced lifetime and revocation; claims alone do not establish permanent access.
## Grounding: patterns that shaped each phase
- **Session fixation, login-CSRF, no-regeneration-on-auth**; OWASP WSTG-SESS-03 / WSTG-SESS-01; the classic ACROS / Mitja Kolšek session-fixation paper. Highest-impact variant: fixing the session of an SSO/admin user.
- **Same-parent-domain CSRF reaching session state**; Argo CD **CVE-2024-22424** (attacker-controlled HTML on the same parent domain; inspect the advisory’s content-type/CSRF preconditions). Use this when a session cookie relies on `SameSite=Lax` as its only CSRF defence.
- **Refresh-token rotation & automatic reuse-detection**; the Auth0/IETF OAuth-Security-BCP model: public-client refresh tokens require rotation with replay detection or sender-constraining; test the implemented protection and replay policy.
- **Device Bound Session Credentials (DBSC)**; the W3C/Chrome DBSC draft binds a session to a TPM/device key. Test the *downgrade*: does the server still accept a non-bound cookie when the DBSC challenge is stripped?
- **Cookie attribute hardening**; OWASP WSTG-SESS-02; `__Host-`/`__Secure-` prefixes per RFC 6265bis. Missing `HttpOnly` can be a hardening finding; high-impact theft claims require an actual applicable attack path (chain with `hunt-xss`/`hunt-dom`).
- **Entropy**; NIST SP 800-63B-4 requires session-binding secrets from an approved random bit generator and at least 64 bits in length. Decodable claims alone are not a finding: signed/encrypted tokens can contain predictable claims while preserving integrity and sufficient unpredictable secrets.
Set-Cookie: __Host-sess=...; Secure; Path=/     # GOOD; hard to fixate
> **Two-session rule.** Every invalidation/fixation claim is proven with TWO concrete sessions captured by a real flow; attacker **A** and victim **B**; never with hardcoded placeholder strings. Helpers below capture real cookies from `curl`'s Netscape jar.
### Phase 1: Session Fixation (regeneration-on-login)
**FP guard:** a value *change* is not automatically safe; some apps rotate the readable cookie but keep a stable server-side session keyed by a second cookie. Diff the FULL `Set-Cookie` set and confirm the *old* value is genuinely dead (Phase 2). Also confirm `/api/me` returns *your* identity, not a generic 200/landing page.
### Phase 2: Invalidation on Logout
# Replay A's OLD cookie value explicitly (do NOT reuse the jar: logout may have
- Don't trust the status code. A cached/edge 200 or a generic SPA shell returns 200 for everyone. **Body-diff** `AFTER` against `BEFORE`; the finding is only real if `AFTER` still contains A's *unique identity marker* (email, user-id, CSRF token, account name).
- Re-test after a **short delay** and from a **different IP**; some servers lazily expire on next access or pin sessions to IP.
### Phase 3: Invalidation on Password / Email Change (persistent-ATO core)
#  that you also control as B: never use a real third party.)
- Repeat the identical flow for **email-change** (`/settings/email`) and for **logout-all-devices**; apps frequently invalidate the *acting* session (B) but not *sibling* sessions (A). That sibling-survival is the exact persistent-ATO primitive `hunt-ato` chains.
### Phase 4: Cookie Attribute Analysis
- **HttpOnly** missing → cookie reachable via `document.cookie`. A hardening observation whose severity requires context; a demonstrated XSS/DOM sink can establish a theft chain (`hunt-xss`/`hunt-dom`); note it, don't report standalone as High.
### Phase 5: Session-ID Entropy
- **Sequential / monotonic**; `sort -n` the decoded values; a steady +1/+N delta = predictable.
- **Decodable structure**; `base64 -d` / hex-decode each ID and look for embedded `userId`, unix timestamps, or PIDs.
- **Bit entropy**; feed the raw bytes to `ent` or `dieharder`; Small samples cannot establish cryptographic entropy; review generation design and the applicable standard. 10 samples is far too few to claim anything; gather hundreds.
- **FP guard:** a long random-*looking* token is not proof of strength; statistical samples cannot prove cryptographic unpredictability; inspect the generation design where possible. Conversely a short token with high per-char entropy may still be fine; measure, don't count characters.
### Phase 6: JWT-as-Session
- **Tampering (alg/kid/key-confusion) is owned by `hunt-api-misconfig`**; hand off `jwt_tool $JWT -T` / `-X a` there rather than duplicating it.
### Phase 7: Refresh-Token Rotation & Reuse-Detection
# 2) Use RT1 to mint a new access token: check the configured protection: rotation or sender-constraining; this example tests rotation only.
### Phase 8: OAuth/SSO Session Linkage & DBSC Downgrade
- **Two real sessions, not placeholders**; every fixation/invalidation claim uses A and B captured by the `curl` flows above.
- **Body-diff, never status-only**; a 200 means nothing without the account's unique identity marker present in the body, diffed against the authenticated baseline.
- **Negative control**; a garbage/random cookie must FAIL where your "surviving" cookie succeeds; otherwise investigate a separate access-control/cache issue rather than asserting session replay.
- **Cache/edge check**; re-request with a cache-buster and from a second IP; rule out an edge-cached or IP-pinned 200.
- **OOB for theft chains**; when the impact depends on exfiltrating a cookie/token (XSS, SSRF, log leak), confirm receipt out-of-band (Collaborator) rather than asserting it.
- **Static-vs-state**; `HttpOnly`/`Secure`/`SameSite` absence is a *policy* observation; only report as High once paired with a real exploit primitive (XSS, network-MITM, CSRF). Standalone attribute gaps are Low/Informational.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.