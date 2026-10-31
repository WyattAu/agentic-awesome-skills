# HUNT-CORS: Cross-Origin Resource Sharing Misconfiguration
Two hard browser rules that kill most "findings"; check these FIRST:
  is sensitive *without* a session (rare); usually this is Informational/Low.
- **Reflect-any-origin + credentials**; server echoes the `Origin` header AND
- **Null-origin trust**; `ACAO: null` + `ACAC: true`. A `sandbox` iframe (or a
- **Subdomain-regex bypass**; trusted-origin regex with a parsing flaw. The
  correct payload depends on *which* flaw (see Phase 3; this is where most
- **Subdomain takeover → trusted origin**; a dangling subdomain that the CORS
- **postMessage missing/loose origin check**; handler that processes
### Phase 1: Discover CORS endpoints
### Phase 2: Reflect-any-origin + null origin
### Phase 3: Subdomain / trusted-origin regex bypass
| `^https?://.*\.target\.com$` | **None**; escaped dot + end-anchor. Correct. | (no simple bypass) | `evil.target.com` is in-scope by design; `x.target.com.evil.com` ENDS in `.evil.com`, fails `$`. Move on or look for subdomain-takeover. |
| `^https?://.*target\.com$` | **Missing dot separator** (no `\.` before `target`) | `https://eviltarget.com` | `.*target\.com$` matches `eviltarget.com`; attacker registers `eviltarget.com`. |
### Phase 3b: Trusted insecure (HTTP) origin
If ACAO reflects/allows any `http://` origin (even a correctly-anchored in-scope one) with ACAC:true, a network attacker on that cleartext host injects a page that reads the authed cross-origin body; no regex flaw needed, the plaintext scheme IS the flaw.
### Phase 4: Pre-flight (OPTIONS) gating bypass
   origin on `OPTIONS` but the actual GET/POST also reflects; the read path is
   directly too; never assume the pre-flight result equals the real-request
### Phase 5: Browser PoCs (the only thing that proves impact)
curl does NOT enforce CORS; it will happily show you a reflected header even
If you see `BLOCKED` / a TypeError, the browser refused the read; it is NOT a
**5b. Null-origin read**; a `sandbox` iframe sends `Origin: null`. The inner
**5c. Trusted-subdomain read**; once you control a host that the regex trusts
### Phase 6: postMessage origin check
postMessage is a separate class from HTTP CORS; impact is DOM-side (XSS,
## Automation (triage only: never the proof)
- **`ACAC: true` alone proves nothing**; it must pair with your reflected
  `target.com.evil.com` against an end-anchored escaped-dot regex; it does not
- **`evil.target.com` reflecting is not automatically a bug**; it is an
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.