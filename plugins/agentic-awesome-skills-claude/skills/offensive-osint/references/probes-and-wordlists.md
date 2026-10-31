### 16.1 Swagger / OpenAPI discovery: 28 paths
### 16.2 GraphQL discovery: 13 paths
### 16.3 High-risk ports: 35 services
### 16.4 Missing security headers: 6 findings
### 16.5 Always-on HTTP checks: 15 paths
Plus parse `/robots.txt` for `Disallow:` paths; those become the next-tier wordlist for that target.
### 16.6 SAML metadata: 5 paths
### 16.7 SSO subdomain prefixes: 8 prefixes
**Tier 1; generic quoted paths:**
Match group: the path. High recall, lots of false positives; apply allowlist downstream.
**Tier 2; API-ish paths (biased filter on tier 1):**
**Tier 3; fully-qualified URLs:**
Selected §16.1–16.12 examples; placeholders must be replaced with approved values and individually confirmed before execution. Examples use `-sS` (show errors, preserve TLS verification) and `-m 10` (10s max). Before any approved capture, set `umask 077`, create a fresh private case directory and use exclusive output paths there; fixed `/tmp` names below are historical placeholders, not safe destinations, `-w '%{http_code}\n'` (print status code), `-A "Mozilla/5.0"` (UA; change per persona).
# Spring Boot /actuator/heapdump (CRITICAL: saves binary; check size)
# AWS (use boto3 instead of curl: pre-signing complexity)
**Historical email-header technique; separate communication approval required:**
Do not send unsolicited messages during reconnaissance. If the owner supplies an approved test message/bounce, its `Received:` headers showing the inbound mail server's actual IP; sometimes co-located with web origin.
**CDN error-page analysis; use existing captures or an approved lab:**
| **Citrix Netscaler / Gateway** | `/vpn/index.html`, `/logon/LogonPoint/tmindex.html`, `/citrix/` | Version in HTML; CVE-2023-3519 (RCE), CVE-2019-19781 (path traversal RCE); both KEV-listed. |
| **F5 BIG-IP TMUI** | `/tmui/login.jsp`, `/mgmt/tm/sys/` | Banner reveals version; CVE-2022-1388 (auth bypass), CVE-2023-46747; KEV-listed. |
| **Pulse Secure / Ivanti Connect** | `/dana-na/`, `/dana-na/auth/url_default/welcome.cgi`, `/api/v1/` | CVE-2024-21887 (KEV), CVE-2023-46805 (KEV); chained command injection. |
| **Microsoft Exchange OWA** | `/owa/`, `/ews/exchange.asmx`, `/ecp/` | ProxyShell (CVE-2021-34473), ProxyLogon (CVE-2021-26855), ProxyNotShell (CVE-2022-41040); all KEV. |
| **Telerik UI** | `/Telerik.Web.UI.WebResource.axd?type=rau` | CVE-2017-9248, CVE-2019-18935; old but still found. |
# (etc: repeat per product)
| **Helm Tiller** (Helm 2; deprecated but found) | 44134 | `helm --host ${IP}:44134 list` | HIGH (Tiller had cluster-admin) |
4. Inspect `Dockerfile` history (`docker history <image>`); sometimes reveals build args or COPY of secrets.
| **TeamCity** | `/login.html`, `/agent.html?agentId=*`, `/admin/admin.html` | `curl -sS -m 10 "${T}/login.html" \| grep -i 'TeamCity'`; version disclosure. CVE-2024-27198 (KEV). |
`autodiscover.<domain>` resolving to Microsoft IP space (`40.96.0.0/13`, `52.96.0.0/14`, `13.107.0.0/16`) is a hosting lead, not proof of M365 Exchange Online tenancy; even when MX records are obscured by Mimecast/Proofpoint/Barracuda inbound filtering. Probe:
- `&from=20200101&to=20231231`; date range.
- `&filter=mimetype:application/json`; only JSON responses (often APIs).
- `&filter=mimetype:application/javascript`; JS bundles.
- `&filter=statuscode:200`; only successful captures.
- `&filter=urlkey:.*api.*`; only URLs containing "api".
- `&collapse=urlkey`; dedup by URL.
- `&collapse=digest`; dedup by content (catches identical pages re-archived).
# Anything indexed (broad sweep: useful for legacy enumeration)
Legacy `.asp` / `.cfm` / `.jsp` URLs often reveal: forgotten admin panels, old user-enum endpoints, legacy auth flows, SQL-injection-prone parameters. Cross-reference with current DNS; many legacy hosts now NXDOMAIN but the URL paths sometimes survive on a renamed host.
**Real-engagement validation:** in an internal smoke test, prefix-sweep found `vpn.`, `api.`, `intranet.`, `staging.`, `support.`, `eapps.`, `eproc.`, `autodiscover.`; all of which crt.sh missed (or returned 502 for). Treat passive + active as **complementary, not alternatives**.