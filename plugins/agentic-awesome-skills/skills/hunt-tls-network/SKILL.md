# HUNT-TLS-NETWORK: TLS/SSL & DNS Security
- **Dangling-CNAME / dangling-A subdomain takeover**; you control content on `target.com` subdomain. Impact and eligibility require program-specific review. (Owned in depth by `hunt-subdomain`; covered here for the TLS/DNS recon angle.)
- **Spoofable DMARC, proven by delivered-to-inbox email**; not "p=none exists" but an actual mail from `ceo@target.com` landing in a real inbox with a passing/none DMARC verdict in the headers.
- **DNS AXFR returning internal hosts**; full internal hostname/IP map. Concrete recon value, often Medium.
- **mTLS / client-cert bypass on an internal service**; reaching authenticated-only functionality without the cert. Real auth bypass = High.
- **Exploited TLS weakness with a working decrypt/MitM PoC**; almost never achievable remotely in 2024-2026 against a patched stack; see Phase 1 caveats.
## Phase 1: TLS/SSL Audit
# Protocol downgrade surface: TLS 1.0/1.1 still negotiable?
**Accuracy / triage notes; do not over-claim TLS bugs:**
- **SWEET32 (CVE-2016-2183)**; 3DES birthday attack. Requires a long-lived TLS session, an on-path attacker, and ~hundreds of GB / hours of same-key traffic. Realistically un-demonstrable in a bug bounty; report only the *support* of 3DES, expect Low/Info.
- **POODLE (CVE-2014-3566)**; SSLv3 CBC padding oracle. Needs **SSLv3 actually enabled**; almost no modern stack offers it. Confirm with `testssl.sh --poodle` (or `nmap --script ssl-poodle`); modern OpenSSL 3.x dropped the `-ssl3` flag. If SSLv3 won't negotiate, there is no POODLE.
- **FREAK (CVE-2015-0204)** and **DROWN (CVE-2016-0800)**; require export-grade RSA / a shared SSLv2 endpoint respectively. Both are pre-conditions you must *prove present*, not assume. DROWN needs SSLv2 reachable on *some* host sharing the cert/key; only within explicit authorized host scope, check SSLv2 with `testssl.sh --drown` (or `nmap --script sslv2-drown`) on separately authorized SAN hosts before claiming it; modern OpenSSL has no `-ssl2` flag.
- **Heartbleed (CVE-2014-0160)**; if you genuinely find an unpatched OpenSSL 1.0.1 leaking memory, that *is* High/Critical with a real PoC (dump containing keys/cookies). Verify with `testssl.sh --heartbleed` and use an owner-approved minimal canary or validation mechanism; do not collect live memory secrets; this is the rare TLS bug worth a full report.
## Phase 2: HSTS Check
## Phase 3: DNS Zone Transfer (AXFR)
## Phase 4: Email Security (SPF/DKIM/DMARC)
echo "$SPF" | grep -q "~all" && echo "[INFO] SPF softfail (~all); may still deliver to inbox"
Do not report "missing DMARC = email spoofing" from `dig` output alone. DMARC `p=none` (or absent) means the **sending domain published no enforcement**; but the **receiving** mail provider (Gmail, M365, the program's own MX) may still junk or reject your spoof based on SPF, its own heuristics, or ARC. The only proof that survives triage is a **message you delivered to a real inbox**.
**Confirmation gate; a spoof PoC is only valid if you can show:**
Severity is **Medium at best**, and only if delivered-to-inbox. Many programs mark email-auth findings OOS outright; check scope first.
## Phase 5: Security Headers Audit
## Phase 6: Certificate Transparency (Subdomain Discovery)
# crt.sh: certificate transparency logs
## Phase 6.5: Dangling Records → Subdomain Takeover (the finding that actually pays)
**Validation gate; resource registration is optional and requires explicit authorization:**
1. Confirm the dangling target is **unregistered/claimable** (the S3 bucket name is free, the Heroku app does not exist, etc.); the provider error fingerprint alone is necessary but NOT sufficient.
## Phase 7: CAA Records (recon signal only: NOT a reportable finding)
- A **missing CAA record does not let any attacker obtain a certificate.** It only means the owner has not *opted into* restricting which CAs may issue. With or without CAA, an attacker still needs to pass Domain Control Validation (HTTP-01 / DNS-01 / email); which requires already controlling the domain, DNS, or web root.
## Phase 8: mTLS Bypass Attempts
# Header-injection bypass: nginx/HAProxy/Envoy commonly terminate mTLS at the edge and
**mTLS-bypass validation; this is a real auth bypass, so prove access, not just a status code:**
- For the bypass-path angle (`/health`, `/metrics`): only a finding if that endpoint exposes **sensitive data** (internal IPs, build secrets, Prometheus metrics revealing internal topology); an empty `200 OK` health probe is not.
| Spoofable DMARC, **delivered to a real inbox** (PoC headers) | **Medium** | Often OOS; check scope; Inbox (not Spam) + delivered required. *Reading `p=none` from `dig` alone = Info, do not file* |
| Missing HSTS on auth subdomain | **Low / Info** | NOT High; exploitation needs an active MitM position you cannot demonstrate remotely; report only with a working downgrade-capture PoC |
# testssl.sh: comprehensive TLS audit
# sslyze: Python TLS scanner
Each finding ships only with the proof listed; never the `dig`/header output alone.
**Severity (conservative; matches the Chain Table):**
- DMARC spoof delivered-to-inbox: Medium (often OOS; verify scope)
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.