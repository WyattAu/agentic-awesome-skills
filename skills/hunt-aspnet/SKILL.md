- **SharePoint farms** (any version; 2013/2016/2019/SE); sign-only ViewState + permissive ToolPane.aspx + anonymous FormDigest creates the CVE-2025-53770 ToolShell precondition chain
- **Telerik UI for ASP.NET AJAX**; `Telerik.Web.UI.WebResource.axd` is a documented RCE sink when keys leak (CVE-2017-11317, CVE-2017-11357, CVE-2019-18935)
- **Classic ASP.NET Webforms enterprise apps**; banking portals, dealer portals, HR systems left on .NET Framework 4.x
- **WCF services** (`*.svc?WSDL`); often forgotten admin endpoints with looser auth than the main app
- **Sitecore CMS**; ViewState + Sitecore-specific deserialization chains (CVE-2021-42237)
- **DotNetNuke (DNN)**; historic ViewState RCE chains
- **Umbraco CMS**; ViewState + custom deserialization sinks
X-AspNet-Version: 4.0.30319          (classic; disclosure on its own)
- `Server: Microsoft-IIS/10.0` (or `/8.5`, `/7.5`); possible IIS fingerprint; headers can be rewritten or spoofed
- `X-AspNet-Version` header; classic .NET Framework (4.x); .NET Core/5+ does NOT emit this
- Cookies with `ASP.NET_SessionId`, `.ASPXAUTH`, `FedAuth`; Forms or claims auth
- `__VIEWSTATE` in form bodies; Webforms (NOT MVC, NOT Razor Pages, NOT Blazor)
- `MicrosoftSharePointTeamServices` header (sometimes stripped by ELB but leaks in `start.aspx` body); SharePoint
   - HTML-entity-encoded payloads (`&lt;script&gt;`; but these don't execute)
   - In path segments (not query); validator scope depends on framework version
11. **SharePoint-specific deserialization paths**; see `hunt-sharepoint` skill for the ToolPane.aspx + anonymous FormDigest + unencrypted ViewState chain.
12. **SafeControl enumeration via reflection.** SharePoint's `Picker.aspx?PickerDialogType=<TypeName>` (and DNN-equivalent endpoints) accept class names and return DIFFERENT error messages for "type exists but not whitelisted" vs "type does not exist." Feed a wordlist of `Microsoft.SharePoint.*.WebControls.*` types to enumerate the SafeControl list; useful for CVE-2019-0604-family hunting.
    title = title.group(1)[:100] if title else "-"
?q=%3cscript%3e            (URL-encoded; depends on validator config)
?q=javascript:alert(1)     (no < at all; passes validator)
**Telerik exploit gate (CVE-2019-18935; requires encryption keys):**
1. **`viewStateEncryption="Auto"` defaults to signed-only on pages without sensitive ViewState data.** Many SharePoint pages are configured this way. The empty marker does not establish whether ViewState is encrypted; applicable exposed keys and context can enable forgery; verify exact runtime/page protections.
| Request validator blocks `<` in querystring | Move payload to Cookie / Referer / JSON body / multipart filename; validator doesn't reach those contexts in classic ASP.NET |
| `SafeControl` whitelist enforced | Inheritance gate (`instanceof PickerDialog`) IS the gate on patched SP; bypass requires finding a SafeControl subclass with a deserialization sink; enumerate via Picker.aspx |
### Scenario A: Unverified SharePoint preconditions
### Scenario B: Telerik fingerprint
### Scenario C: trace.axd + elmah.axd both exposed on enterprise HR portal
- **`hunt-rce`**; ViewState deserialization is the headline ASP.NET RCE path; signed-only ViewState with exposed applicable keys can enable code injection under additional page/runtime/gadget preconditions. Chain primitive: ASP.NET ViewState dual-parser MAC-bypass anti-pattern detected (signed but not encrypted, `<%@ Page enableViewStateMac="true" viewStateEncryptionMode="Never" %>`) + machineKey recovered (from web.config disclosure, `elmah.axd`, source leak, or GitHub) → `hunt-rce` ysoserial.net `TypeConfuseDelegate` gadget → arbitrary command in `w3wp.exe` worker-process identity.
- **`hunt-sharepoint`**; SharePoint farms inherit every ASP.NET anti-pattern plus their own surface. Chain primitive: ASP.NET fingerprint reveals SharePoint (X-SharePoint headers + `/_layouts/` reachable) → pivot to `hunt-sharepoint` for SP-specific RCE paths (ToolShell, SafeControl reflection) before generic ViewState attack.
- **`hunt-ntlm-info`**; IIS sites that advertise NTLM/Negotiate anonymously leak AD topology. Chain primitive: ASP.NET app behind IIS with `WWW-Authenticate: NTLM` → `hunt-ntlm-info` Type-2 challenge capture → internal forest name → cross-reference Entra tenant via `m365-entra-attack` discovery.
- **`hunt-file-upload`**; Telerik RadAsyncUpload, Kentico, Umbraco, and DotNetNuke all have historical upload-handler RCE. Chain primitive: ASP.NET CMS fingerprinted → `hunt-file-upload` bypass matrix against the CMS upload handler → `.aspx` written into web-accessible path → request → RCE under app-pool identity.
- **`triage-validation`**; `trace.axd`/`elmah.axd` disclosure is only Critical when it actually leaks live credentials/tokens; pure stack traces are usually Low. Chain primitive: pull every reported finding through `triage-validation` 7-Question Gate before submission; distinguish "verbose error" (informational) from "live bearer token in error log" (Critical) before writing the report (`redteam-report-template`).
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.