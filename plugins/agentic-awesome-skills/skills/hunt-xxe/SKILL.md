- **Domains with file-read capability**; `/etc/passwd`, `/etc/shadow`, internal config files, AWS metadata endpoints
- **Subdomains sharing backend infrastructure**; one XXE endpoint can pivot to internal services across dozens of domains (as demonstrated by 26+ Uber domains via a single entry point)
- **API gateways** accepting XML content types; especially REST APIs that silently accept `Content-Type: application/xml`
- **File upload features**; SVG, DOCX, XLSX, PDF, PPTX parsers on the server side
- **SAML/SSO endpoints**; SAML assertions are XML-based; actual parser and signature configuration determines exposure
- **Office/document processing services**; any feature that converts or processes user-supplied documents
- **Java stacks**: Spring, Struts, JAX-WS; default XML parsers (SAX, DOM) are XXE-vulnerable without explicit hardening
- **PHP**: `simplexml_load_string()`, `DOMDocument::loadXML()`; vulnerable by default pre-PHP 8
- **WSDL/SOAP services**: Always test; legacy XML parsing virtually guaranteed
1. **Map every XML entry point**; Use Burp Suite passive scanner to flag all requests/responses with XML content types. Also intercept JSON endpoints and manually swap `Content-Type` to `application/xml` with equivalent XML body.
2. **Identify file upload features**; Upload SVG, DOCX, XLSX, and observe if the server processes/renders content. These are often XML under the hood.
3. **Attempt inline XXE (classic file read)**; Replace the XML body with a basic entity test payload targeting `/etc/passwd` or `C:\Windows\win.ini`. Observe if the value is reflected in the response.
4. **If no reflection, pivot to Blind OOB**; Set up an OOB listener (Burp Collaborator, interactsh, or a self-hosted netcat server). Inject an external entity pointing to your callback URL. Confirm DNS/HTTP hit to validate the parser is making outbound connections.
5. **Bound proof**; use an owner-created nonsensitive canary file or unique callback; stop before real data exfiltration.
6. **Assess potential SSRF separately**; only a scope-approved controlled endpoint; timing/error differences alone are inconclusive. Do not request metadata credentials or internal admin resources.
7. **Test all subdomains sharing the same backend**; If one subdomain is vulnerable, enumerate and test all others systematically. Shared infrastructure is a hypothesis, not proof; do not expand target scope.
8. **Review schema-valid XML text locations**; entity references cannot replace element names, and external entities are not permitted in attribute values. Use a controlled lab document with text-node substitution.
9. **Test for error-based exfiltration**; If OOB is blocked, trigger XML parsing errors that include file content in the error message returned to the client.
10. **Document the full impact chain**; Demonstrate: file read → SSRF → internal service access → note which internal domains/IPs are reachable.
Needs zero network egress. Local DTD paths differ per distro (yelp/docbookx is common); enumerate. (PortSwigger: XXE by repurposing a local DTD.)
### Blind OOB: DNS/HTTP Callback
### Blind OOB: File Exfiltration via Parameter Entity (two-stage)
### DOCX/XLSX XXE: Inject into `[Content_Types].xml` or `word/document.xml`
1. **Parser/configuration/version combinations**; defaults differ substantially; inspect the exact library, resolver, entity flags and runtime version instead of inferring XXE from a language name.
2. **Framework upgrades without security re-review**; Older versions of Spring, Struts, and similar frameworks enabled XXE by default; developers didn't re-audit XML handling when libraries changed.
3. **Hidden XML consumption**; Developers accept JSON at the API layer but convert to XML internally, or use libraries (Apache POI, python-docx) to process uploads without realizing those formats are XML containers.
4. **Copy-paste code from StackOverflow**; XML parsing examples online rarely include entity disabling. Developers copy minimal working examples straight into production.
5. **SAML/SSO library misconfigurations**; SSO integrations often delegate XML parsing to third-party libraries with XXE enabled; developers assume "library handles security."
6. **Testing gaps on non-primary content types**; QA tests JSON APIs extensively; XML code paths receive minimal security testing because they're secondary or legacy.
7. **Microservice XML messaging**; Internal service-to-service communication uses XML (SOAP, custom schemas) and is treated as a "trusted internal" concern, bypassing security review.
XXE classic payloads (`<!ENTITY xxe SYSTEM "file://...">`) are not universally exploitable in 2026. Most mainstream language ecosystems have hardened their default XML parsers since 2018-2024. Fingerprint the target stack BEFORE investing time in XXE; sometimes the parser is already safe and the bug class doesn't apply.
| .NET `XmlReader` with `DtdProcessing=Parse` + `XmlResolver` explicitly set | Expands | **YES** (default since .NET 4.5.2 is `DtdProcessing.Prohibit`, which THROWS on any DOCTYPE; modern default is safe) |
| Python `lxml` (verified 6.x; likely ≥ 5.x) | Drops SYSTEM file content even with `resolve_entities=True`; may still issue network fetches when `no_network=False` | NO for file read (verified locally on 6.1.0; see `docs/verification/phase2g-saml-mfa-xxe.md`) |
- Server says nothing identifiable + endpoint accepts XML → **probe with `&hello;` first**; if the inline entity expands, escalate to SYSTEM. If the inline entity also fails to expand, the parser may be hardened; pivot.
A tester discovered an XML parsing endpoint on one Uber subdomain. The backend processed XML using a vulnerable parser, and the server made outbound HTTP requests to attacker-controlled infrastructure (blind OOB). Because the vulnerable XML processing service was a shared internal microservice, the same vulnerability was reachable through 26+ different public-facing domains; all sharing the backend. A single payload could read `/etc/passwd`, internal config files, and reach AWS metadata endpoints, effectively giving access to credentials reusable across the entire infrastructure.
A REST API endpoint nominally accepting JSON was found to also parse requests submitted as `application/xml`. The underlying service converted XML to an internal format using an unpatched Java `DocumentBuilderFactory`. By injecting a SYSTEM entity pointing at `http://10.0.0.x` address ranges, the tester could probe internal services, reach an unauthenticated internal admin panel, and retrieve AWS instance metadata including temporary IAM credentials; all through a single API call requiring only a valid session token.
## Disclosed Report Citations (Backfill +6: 2016-2024)
The following real, verified bug-bounty / coordinated-disclosure cases extend this skill. Three cases (#7, #9, #10) are OOB-required (blind); they cross-reference the `hunt-ssrf` OOB-Or-It-Didn't-Happen Gate and cannot be claimed without a Collaborator/interactsh callback.
5. **Mail.ru; Pulse RSS/Atom feed XXE (file read)** ([H1 #505947](https://hackerone.com/reports/505947))
    - Year: 2019; **$6,000**
6. **Twitter; sms-be-vip.twitter.com SOAP/SXMP servlet XXE** ([H1 #248668](https://hackerone.com/reports/248668))
    - Subclass: SOAP XXE (cloudhopper SXMP servlet); confirmed via HTTP callback **and** reflected file read
    - Year: 2017; **$10,080** (classic SOAP-XXE reference case)
7. **Zivver; SVG profile-picture upload XXE → SSRF** ([H1 #897244](https://hackerone.com/reports/897244))
    - Year: 2020; Zivver had no paid program at the time
8. **Open-Xchange; Blind XXE via PowerPoint (.pptx) import** ([H1 #334488](https://hackerone.com/reports/334488))
    - Year: 2018; **$2,000**
9. **Uber; Blind OOB XXE on ubermovement.com** ([H1 #154096](https://hackerone.com/reports/154096))
    - Year: 2016; **$500**
10. **Adobe Commerce / Magento; "CosmicSting" CVE-2024-34102** ([Assetnote writeup](https://www.assetnote.io/resources/research/why-nested-deserialization-is-harmful-magento-xxe-cve-2024-34102))
    - Subclass: XXE-to-RCE (nested deserialization → XXE in Laminas request body); modern, exploited in the wild
    - Year: 2024; Adobe HackerOne bounty paid, CVSS 9.8
- **`hunt-ssrf`**; XXE is SSRF-with-XML-syntax once you discover the parser fetches external entities. Chain primitive: blind XXE OOB `<!ENTITY % x SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/role">` → exfil AWS IMDS creds via parameter-entity DTD callback → STS AssumeRole chain. Where pure SSRF is filter-blocked, `gopher://` via XXE-in-`libxml2` can still reach Redis/SMTP.
- **`hunt-file-upload`**; XXE most often hides inside file-upload features that quietly parse XML (DOCX/XLSX/PPTX, SVG, GPX, KML, OOXML, SOAP attachments). Chain primitive: upload DOCX with malicious `[Content_Types].xml` containing parameter-entity DTD → OOB file read of `/etc/passwd` / `web.config` / `.aws/credentials` via the document parser running server-side.
- **`hunt-rce`**; XXE → RCE is rare but real on PHP (`expect://`), Java (XSLT extensions with `<xsl:script>`/Xalan), and older XmlSpy/SAXON deployments. Chain primitive: XXE in a Java endpoint using a vulnerable XSLT processor → `<xsl:value-of select="rt:exec(rt:getRuntime(),'id')"/>` → RCE; or PHP XXE with `expect://id` stream wrapper enabled → direct RCE.
- **`hunt-sharepoint`**; On-prem SharePoint/Exchange/IIS stacks have a long history of XXE in SOAP/CSOM/EWS endpoints. Chain primitive: anonymous XXE in `_vti_bin/*.asmx` or EWS SOAP → read `web.config` → recover `<machineKey>` → ViewState deserialization → RCE (the ToolShell-adjacent precondition).
- **`security-arsenal`**; Reach for the XXE payload tree: standard SYSTEM file read, parameter-entity OOB DTD pattern (the `%` indirection that makes blind XXE work), `php://filter/convert.base64-encode/resource=` for binary-safe read, XInclude (`<xi:include href=...>`) when DOCTYPE is blocked, billion-laughs/quadratic-blowup for DoS, and the `jar://` / `netdoc://` Java-specific wrappers.
- **`triage-validation`**; Apply the Reproducibility + Pre-Severity Gates. An XXE that only triggers an OOB DNS callback with NO data exfil is Low/Medium (information disclosure of "this parser fetches entities"), not Critical. Critical needs proof of file read or internal HTTP; show the actual `/etc/passwd` content or the IMDS JSON response in the report.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.