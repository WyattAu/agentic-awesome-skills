- **SSO/SAML implementations** at enterprise SaaS companies (Slack, Okta, OneLogin integrations); payouts regularly in the $5K–$25K+ range
- **Admin panels and partner/internal portals**; subdomain-separated admin surfaces like `partners.shopify.com`, `admin.company.com`
- **Third-party auth plugin integrations**; WordPress plugins (OneLogin, WP-SAML-Auth), Drupal SSO modules, any CMS with pluggable auth
- **XMLRPC endpoints** on WordPress; often forgotten, bypasses standard WP auth flows entirely
- **OAuth callback flows**; state parameter mishandling, redirect_uri mismatches
- **API authentication layers**; especially where auth was bolted on after the fact
When an org or instance registers an SSH certificate authority, cert `principals` may not be bound server-side to the requesting identity; a member can mint a cert asserting another user's principal and authenticate as them (e.g. modify another user's resource given only its URL). Add SSH-CA trust to the auth-surface list on Git-hosting targets; niche/platform-specific. Disclosed: reports/1901040.
   - XMLRPC uses WordPress-native credentials, not SSO; test with `system.listMethods` first, then `wp.getUsersBlogs`
| **SharePoint (any version)** | `/_vti_bin/Authentication.asmx` (`Mode` + `Login` SOAP ops) | Native Forms-auth credential; FedAuth cookie returned; no rate limit on this endpoint observed on SP2013 farms; **this is the canonical SP equivalent of the WP XMLRPC bypass** |
**Lesson from a authorized engagement:** A an enterprise dealer portal on SharePoint 2013 had a custom branded `customlogin.aspx`. The hunt-auth-bypass skill was loaded but the matrix above did not exist in this document; and the WordPress XMLRPC pattern was not connected to the SharePoint equivalent. `/_vti_bin/Authentication.asmx` was reachable anonymously, accepted unlimited credential attempts with no rate limit and no lockout, and was the highest-impact finding in the engagement. Walking this matrix on the first pass would have surfaced it immediately.
1. **SSO bypasses local auth entirely at the UI layer, but not at the API layer**; developers disable the login form but forget that API endpoints (`/xmlrpc.php`, REST API, mobile API) have their own auth handlers that still accept native credentials.
2. **SAML signature validation is skipped or optional**; library defaults often don't enforce signature checking; developers use `wantAssertionsSigned: false` or fail to configure the IdP certificate correctly.
3. **Shared session infrastructure across different trust levels**; partner portals and admin portals reuse the same session cookie or JWT secret because they're built on the same internal framework, assuming access control at the application layer is sufficient.
4. **Trust inheritance in multi-tenant architectures**; a token issued in a lower-privilege context (partner, reseller) is accepted in a higher-privilege context because the verification only checks signature validity, not the issuance context.
5. **Plugin/module auth is independent of application auth**; every WordPress plugin that handles auth (contact forms, REST API extensions, WooCommerce) may implement its own auth handler inconsistently with the main site's SSO.
6. **XML parsing inconsistencies**; different XML parsers (used by SP vs. IdP) handle comments, namespaces, and whitespace differently, enabling confusion attacks where the signed content differs from the evaluated content.
| Separate session stores per portal | Check if cookie domain is `.target.com` (wildcard); cookie bleeds between subdomains |
**Scenario 1; SSO Enforcement Bypassed via Forgotten Protocol Endpoint**
**Scenario 2; SAML Assertion Forgery via Signature Validation Failure**
A major enterprise communication platform's SAML SP implementation failed to properly validate assertion signatures in specific edge cases. By manipulating the XML structure of a captured SAMLResponse (specifically through comment injection or namespace prefix attacks), an attacker could modify the `NameID` value to impersonate any user in an organization; including workspace administrators; without possessing that user's credentials or private key material. Impact: Complete account takeover of any user within a SAML-enabled organization; attacker gains access to all messages, files, and integrations in the workspace.
**Scenario 3; Cross-Portal Privilege Escalation via Shared Auth Backend**
An e-commerce platform's partner/reseller portal issued authentication tokens that were validated by the same backend service as the merchant admin portal. A partner-level account (lower trust, external-facing) could use its issued credentials or tokens to authenticate directly against admin-tier API endpoints, bypassing the merchant onboarding and permission assignment flow. Impact: A malicious partner could access any merchant's admin panel, modify store configurations, exfiltrate customer PII and payment data, or install malicious scripts; affecting thousands of merchant storefronts.
## Disclosed Report Citations (Backfill +8: 2016-2025)
5. **GitHub Enterprise Server; SAML XSW via parser differential (CVE-2025-25291/25292)** ([H1 #2579939](https://hackerone.com/reports/2579939) · [Blog](https://github.blog/security/sign-in-as-anyone-bypassing-saml-sso-authentication-with-parser-differentials/))
    - Year: 2025; GitHub Security Lab bounty (program max class, internally rated Critical)
6. **GitHub Enterprise; SAML signature bypass on encrypted assertions (CVE-2024-4985)** ([H1 #2475347](https://hackerone.com/reports/2475347) · [ProjectDiscovery advisory](https://projectdiscovery.io/blog/github-enterprise-saml-authentication-bypass))
    - Year: 2024; bounty undisclosed, CVSS 10.0
7. **Uber; SAML auth bypass on `uchat.uberinternal.com`** ([H1 #223014](https://hackerone.com/reports/223014))
    - Year: 2017; **$8,500**
8. **Uber; OneLogin SSO bypass via WordPress XMLRPC** ([H1 #138869](https://hackerone.com/reports/138869))
    - Subclass: WordPress XMLRPC bypassing SSO (legacy-auth path not gated); canonical Legacy-Protocol Matrix case
    - Year: 2016; **$7,000**
9. **Slack; SAML "confused-deputy" assertion reuse** ([Writeup](http://blog.intothesymmetry.com/2017/10/slack-saml-authentication-bypass.html))
    - Year: 2017; **$3,000**
10. **HackerOne; SAML signup domain enforcement bypass via control characters** ([H1 #2101076](https://hackerone.com/reports/2101076))
    - Year: 2024; bounty awarded (amount undisclosed)
11. **8x8 / Jitsi-Meet; JWT alg-confusion (asymmetric verifier accepts symmetric alg)** ([H1 #1210502](https://hackerone.com/reports/1210502))
    - Year: 2021; bounty undisclosed
12. **Argo CD (Internet Bug Bounty); JWT audience claim not validated (CVE-2023-22482)** ([H1 #1889161](https://hackerone.com/reports/1889161))
    - Year: 2023; **$2,400** via IBB
- [Duende BFF; Token-Confusion & Session-Fixation (2024-2026 surface)](references/details.md)
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.