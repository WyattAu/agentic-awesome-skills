- **SP2013 farms still on the public internet**; check actual affected code/patch exposure; do not assign critical severity from EoL alone.
- **Dealer / partner / supplier portals** built on SharePoint by enterprise integrators (German VW group, a enterprise system integrator, etc.); high-impact business data, often nested inside corporate AD trees.
- **SharePoint farms with anonymous Forms-auth zones**; Authentication.asmx becomes anonymously brute-forceable.
- **SharePoint inside corporate AD parent forests**; NTLM Type-2 leak (see `hunt-ntlm-info`) discloses the parent forest membership.
- **Telerik-integrated SharePoint installations**; additional deserialization sinks on top of SP's own.
SPRequestGuid: <GUID>                           (always; anonymous and authenticated)
Set-Cookie: ASP.NET_SessionId=...               (always; IIS session)
/_layouts/15/                  (SP2013+ layouts root; SP2010 used /_layouts/ without the 15)
/_layouts/14/                  (legacy SP2010; almost EoL since 2020-10-13)
/_vti_pvt/                     (FrontPage-RPC config; usually 403)
/_api/$metadata                (OData metadata; often anonymous + large)
/_api/contextinfo              (FormDigest issuer; POST only)
/_catalogs/users/simple.aspx   (user list; usually 403)
/_layouts/15/start.aspx        (anonymous landing; leaks version)
/_layouts/15/ToolPane.aspx     (web part editor; ToolShell sink)
/_layouts/15/Picker.aspx       (people/list picker; SafeControl recon)
/_layouts/15/download.aspx     (SP-internal file resolver; NOT outbound SSRF)
/_layouts/15/error.aspx        (error page; anonymous)
/_layouts/15/AccessDenied.aspx (denied page; anonymous)
/_layouts/15/<Customer>/       (custom-branding modules; see Methodology step 8)
/_vti_bin/Authentication.asmx  (THE legacy login bypass; see hunt-auth-bypass Legacy-Protocol Matrix)
/_vti_bin/sts/                 (Security Token Service; usually 302 to error)
__REQUESTDIGEST                                            (CSRF token; leaks even to anon if endpoint mis-configured)
__VIEWSTATEENCRYPTED=""                                    (Sign-only ViewState; see hunt-aspnet)
   /_layouts/15/SignOut.aspx                               → logout; anonymous OK
   /_vti_bin/Authentication.asmx                           → legacy SOAP login; anonymous brute-force (CRITICAL)
   /_vti_bin/Authentication.asmx?WSDL                      → WSDL; confirms Login + Mode ops
   # Should return 200 OK: server treats anonymous-with-digest as authorised state-changing POST.
7. **`download.aspx` is NOT outbound SSRF; recognize and don't waste time.** SP's `/_layouts/15/download.aspx?SourceUrl=` is an **SP-internal path resolver**, not a generic URL fetcher. Behaviours:
   - External URL (`http://evil.example.com/x`) → 500 with `"The Web application at <URL> could not be found"`; server tried to resolve as an SP web app, didn't fetch.
   - Same-origin file URL → 500 with `"<nativehr>0x81070211</nativehr>...Cannot open file '<path>'"`; server tried SPFile.OpenBinary, file not found.
   - `file://`, UNC paths, `gopher://`, etc. → 500 with `"Value does not fall within the expected range"`; URL-scheme validator rejects.
9. **Search service probe.** `/_api/Search` returns a small JSON descriptor anonymously. `/_api/search/query?querytext='X'` returns 500 with stack trace if the Search Service Application is not running; useful infra disclosure but not directly exploitable.
    - `/_api/web/Lists`; enumerate lists
    - `/_api/web/SiteUsers`; enumerate users
    - `/_api/web/getfolderbyserverrelativeurl('/Shared Documents')/Files`; file enumeration
    - `/_layouts/15/people.aspx`; user listing
    - Custom customer-branded modules; check for IDOR, business logic
- `<ErrorCode>PasswordNotMatch</ErrorCode>` → auth fail (uniform for non-existent users too; no enum leak via error string)
2. **`viewStateEncryption="Auto"` on layouts pages.** SharePoint's default ViewState mode signs-only for pages without sensitive form fields. Pages like ToolPane.aspx have `__VIEWSTATEENCRYPTED=""`; exploitable if machineKey leaks.
6. **`<SafeControl>` whitelist in web.config trusted as the only gate.** Picker.aspx enforces an `instanceof PickerDialog` check, which is patched against the original CVE-2019-0604 vector; but the underlying SafeControl model itself is anonymously enumerable via the Picker.aspx error differential.
9. **Custom-branding module (`/_layouts/15/<Customer>/`) JS bundles loaded with `?v=YYYYMMDD` query strings.** The query string may be a cache-busting/build value, not a last-modified date; useful for "this app is actively maintained" vs "this app is abandoned" determination.
| Branded `customlogin.aspx` with lockout / CAPTCHA / MFA | `/_vti_bin/Authentication.asmx` legacy SOAP; none of those protections apply |
| `WWW-Authenticate: NTLM` requires authenticated callers | Default IIS `extendedProtection=None` lets you elicit the Type-2 challenge anonymously; see `hunt-ntlm-info` |
| Anonymous `__REQUESTDIGEST` issued (ToolShell precondition) | Pair with anonymous ToolPane POST + unencrypted ViewState; the deserialization sink yields RCE without machineKey; machineKey is then dumped by the shell for persistent re-exploitation |
| Custom error pages set | Trigger different code paths (XML-shaped ViewState → dual-parser error differential; see `hunt-aspnet`) |
| WAF blocks `<` in query | Move payload to Cookie / Referer / SOAP body; request validator doesn't reach those contexts |
| HTTP/2 H2.CL smuggling | AWS ALBs often don't advertise `h2` ALPN; close that family early via `openssl s_client -alpn h2,http/1.1` |
### Scenario A: a authorized SharePoint engagement against an EoL on-prem farm
Target: `https://target-portal.example/`; SharePoint Server 2013 build `15.0.5545.1000` (KB5002381 / final EoL April 2023 CU). Tenant = a system-integrator tenant (Swiss <ParentCorp> importer) inside a corporate global AD (`customer.parent-corp.example`). Server hostname `WIN-XXXXXXXXXXX` (default Windows installer pattern).
1. **Authentication.asmx anonymous credential brute-force**; 10-burst test showed uniform 0.6-0.9 s timing with no rate limit, no lockout, no CAPTCHA, no MFA challenge. Identical 431-byte responses.
2. **HTTP request smuggling TE.CL**; 5/5 trials showed 12.18 s back-end hang vs 0.16 s baseline. 4 additional obfuscation variants (space-before-colon, trailing-space, lowercase, mixed-case) showed 6.16 s hang; each bypassing simple WAF rules.
3. **ToolShell precondition chain**; anonymous GET ToolPane.aspx (200) + anonymous POST `/_api/contextinfo` (200, valid FormDigest) + anonymous POST ToolPane.aspx with digest (200, no auth challenge) + `__VIEWSTATEENCRYPTED=""`. Permanent zero-day on EoL SP2013.
### Scenario B: `/_layouts/15/download.aspx?SourceUrl=` recognized correctly as NOT-SSRF (saved-time example)
### Scenario C: CVE-2019-0604 patch verification via Picker.aspx
Same target. Feeding `Microsoft.SharePoint.WebPartPages.DataFormWebPart` (the canonical CVE-2019-0604 deserialization gadget) to Picker.aspx returned `"Only PickerDialog types can be used with the dialog. The type should be configured as a safecontrol in this site."`; meaning the type EXISTS and is reachable through reflection, but the dialog framework's `instanceof PickerDialog` patch correctly rejects it. The patch IS in place for the original CVE-2019-0604 vector. The class-existence enumeration itself becomes recon for any future CVE-2019-0604-family chain that doesn't go through the inheritance gate.
- **`hunt-auth-bypass`**; Legacy SOAP `/_vti_bin/Authentication.asmx` accepts anonymous Login calls on misconfigured farms. Chain primitive: SharePoint anon SOAP login probe → if response yields cookie or success differential → `hunt-auth-bypass` brute-force matrix (username enumeration via timing, password spray with low-and-slow against the same SOAP endpoint that bypasses ADFS-level lockout) → valid cred → `/_layouts/15/` authenticated surface.
- **`hunt-ntlm-info`**; Every SharePoint farm advertises `WWW-Authenticate: NTLM` anonymously on `/_vti_bin/`. Chain primitive: SharePoint NTLM Type-2 challenge capture → `hunt-ntlm-info` AV_PAIR decode yields NetBIOS domain + internal DNS forest + DC hostname → feed domain into `m365-entra-attack` ROPC user-enumeration spray on tenant tied to that domain.
- **`hunt-aspnet`**; SharePoint is ASP.NET Webforms under the covers; ViewState, machineKey, and SafeControl reflection all apply. Chain primitive: SharePoint version disclosure → confirm patch level missing → `hunt-aspnet` ViewState dual-parser MAC-bypass → deserialization gadget → RCE in `w3wp.exe` as farm account.
- **`hunt-rce`**; ToolShell precondition chain (CVE-2025-53770) is the current high-impact SP RCE path. Chain primitive: ToolShell preconditions met (`/_layouts/15/ToolPane.aspx?DisplayMode=Edit` reachable via the CVE-2025-49706 auth bypass; a crafted `Referer` header pointing at ToolPane.aspx; + version vulnerable) → `hunt-rce` deserialization gadget chain → SYSTEM/farm-account shell → `m365-entra-attack` lateral via stolen on-prem service-account token to Entra-synced identity.
- **`triage-validation`**; SharePoint farms generate a lot of "looks like a finding" hygiene noise (FormDigest issuance, version disclosure, extension blocklist quirks). Chain primitive: run every SP finding through the 7-Question Gate before reporting; most version-disclosure-only findings die at "is this actually exploitable on this farm" without a paired CVE PoC.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.