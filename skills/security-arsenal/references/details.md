{{7*'7'}}      → 7777777 = Jinja2 (not Twig; Twig gives 49)
### CL.TE: Content-Length front-end, Transfer-Encoding back-end
### TE.CL: Transfer-Encoding front-end, Content-Length back-end
### TE.TE: Both support Transfer-Encoding, obfuscate to disable one
### H2.CL: HTTP/2 front-end with Content-Length injection
# SAMLRaider (Burp extension): most automated XSW testing
Pre-account takeover (usually; requires very specific conditions)
## POTENTIAL CHAINS: VERIFY PREREQUISITES
- **`hunt-xss`** / **`hunt-ssrf`** / **`hunt-sqli`** / **`hunt-ssti`** / **`hunt-idor`**; When a hunter is actively testing a parameter and needs payloads. Workflow primitive: this skill is the payload library those hunt-* skills reach for; the hunt-* skill identifies the sink, this skill provides the syntax.
- **`triage-validation`**; When deciding if a finding is reportable at all. Workflow primitive: the "Always Rejected" and "Conditionally Valid; Requires Chain" tables in both skills must agree; `triage-validation` runs the 7-Question Gate, this skill provides the chain-required mapping used by Q7.
- **`web2-recon`**; When the URL set has been classified by `gf` patterns. Workflow primitive: `gf xss/ssrf/sqli` outputs from recon → look up the corresponding payload section here; `gf` pattern names index directly into this skill's payload sections.
- **`evidence-hygiene`**; When a payload produces output worth screenshotting. Workflow primitive: after a payload demonstrates impact (cookie theft, data exfil), hand off to `evidence-hygiene` for redaction before the screenshot becomes evidence.
- **`bb-methodology`**; When Phase 3 (Discovery) routes by input type. Workflow primitive: Phase 3's decision flow ("ID param → IDOR checklist", "URL input → SSRF checklist") names which section of this arsenal to load.
1. **Encoding tricks**; case variation (`SeLeCt`), URL-encode once, URL-encode twice, Unicode escape (`<`), HTML-entity (`&#x3c;`), UTF-8 overlong sequences.
2. **Parser quirks**; XML namespace, JSON `\u` escapes mid-keyword, `Content-Type: application/json` vs `application/x-www-form-urlencoded` parser-confusion, multipart boundary tricks.
3. **Protocol-level**; HTTP/2 vs HTTP/1.1 (some WAFs only inspect one), Host header injection, `X-Original-URL`, `X-Forwarded-*` smuggling.
4. **WAF rule-specific bypasses**; Cloudflare, AWS WAF, Akamai, Imperva, F5 ASM each have known signature gaps; load the vendor-specific payload subsection.
Most engagements end at step 2; modern WAFs trip on the parser-quirk class because the WAF and the origin app disagree on what's a "valid" request.
Generic words appear naturally in target content. A search for `javascript` hitting "JavaScript Tutorial" is not reflection; it's keyword overlap. Use unique random strings:
# now m is like "K7gXq2pNRm1z": search for THIS in the response
If the marker appears in the response, you have reflection. If it appears unescaped in HTML context, you have XSS potential. If it appears in a Location header, redirect. If it appears in a SQL error, injection. The marker is the single source of truth; generic keywords lie.
Same rule applies to blind boolean oracles where the diff is response-length or status-code under jitter; sample, don't assume.