- Internal-network vCenter (out of scope; external boundary discipline)
## Step 1: Version fingerprinting
## Step 2: CVE matrix (external-exploitable, sorted by historical impact)
| **CVE-2024-37085** | ESXi 7.0/8.0 < specific patch | AD group "ESX Admins" auto-admin bypass | High; Domain takeover→ESXi RCE, exploited in ransomware ops |
| **CVE-2024-22273** | ESXi/Workstation/Fusion storage controller | OOB read/write; requires VM-local access (NOT a pre-auth network SSRF; see Section 9) | Important (CVSS 8.1), not external pre-auth |
## Step 3: CVE-2021-21972 probe (still common on stale appliances)
# Detection only: DO NOT execute the file upload without explicit scope OK
## Step 4: CVE-2022-22954 (Workspace ONE SSTI) probe
# Stage A: detection only: reachability + baseline. No command execution yet.
## Step 5: Authentication boundary
## Step 6: SSO / vmdir LDAP enumeration
## Step 7: Managed Object Browser (MOB): frequently leaks data
## Step 8: vSphere REST API enumeration (post-cred)
# Datastore file download (HUGE: VMDK files, snapshots, credentials in cloud-init)
## Step 9: Workspace ONE Access specific paths
## Step 10: Aria / vRealize specific paths
- **`Greenbone/openvas-scanner` VMware NASL plugins**; version detection
- Excessive 404s on `/ui/vropspluginui/*`; IDS signature
Pair with `mid-engagement-ir-detection` skill; vCenter is monitored heavily in mature SOCs.
Internet-exposed vCenter is unfortunately common on the perimeter; and frequently outdated by years. The 2021-21972 / 21985 / 22954 trifecta still pays in 2026 because patching cycles for hypervisor management are slow and vendor-managed.
- **DO NOT spray vCenter SSO**; lockout is aggressive; one chance often
- **DO NOT execute file-upload PoCs without explicit OK**; they create persistent webshells; cleanup overhead and audit trail
- **DO NOT confuse ESXi-management-on-internet with vCenter**; different attack surfaces; ESXi Open SLP CVEs target port 427
- **DO NOT skip SSL handshake banner check**; VMware exposes versions there; this is the lowest-noise initial probe
- `enterprise-vpn-attack`; vCenter is frequently the post-VPN target; if VPN is breached, vCenter is the natural next pivot (but internal; defer to user)
- `m365-entra-attack`; vCenter SSO sometimes federated to Entra; cred-chain bridging
- `mid-engagement-ir-detection`; vCenter monitoring is sensitive; expect mid-engagement mitigations
- `redteam-report-template`; vCenter findings need clear blast-radius framing (this is the virtualization plane, not just an app)
- **`hunt-saml`**; vCenter Workspace ONE / VMware Identity Manager publishes SAML SP metadata at `/SAAS/API/1.0/GET/metadata/idp.xml` and consumes assertions at predictable ACS URLs. Chain primitive: vCenter SAML SP metadata reachable → IdP fingerprinted → `hunt-saml` XSW1-XSW8 against the federating IdP → forged assertion with `userPrincipalName=administrator@vsphere.local` → SP-impersonation as vCenter admin → full virtualization-plane takeover.
- **`hunt-rce`**; VMware's high-impact CVE catalog (CVE-2021-21972, CVE-2021-21985, CVE-2022-22954, CVE-2023-20887) is almost entirely pre-auth RCE. Chain primitive: vCenter version fingerprint via SSL banner or `/ui/login` body → confirm patch level missing → `hunt-rce` deserialization/SSTI gadget from the matching CVE PoC → `root` on vCenter appliance → API-token mint → cluster-wide VM control.
- **`enterprise-vpn-attack`**; VPN compromise + vCenter on internal-only is a natural post-VPN pivot, but external-only engagement scope sometimes forbids it. Chain primitive: VPN appliance CVE → foothold inside corp network → if scope permits, `vmware-vcenter-attack` becomes reachable on internal-only vCenter → datacenter takeover.
- **`m365-entra-attack`**; Some VMware deployments federate vCenter SSO to Entra. Chain primitive: vCenter SSO discovery → AuthURL points to `login.microsoftonline.com` → `m365-entra-attack` Entra ATO on `administrator@vsphere.local` synced identity → SAML assertion → vCenter admin without ever brute-forcing vCenter SSO.
- **`mid-engagement-ir-detection`**; VMware vSAN/vCenter alerting is sensitive; expect SOC to patch or block within hours of detection. Chain primitive: confirmed vCenter CVE → run `mid-engagement-ir-detection` baseline capture BEFORE attempting exploitation → if response patterns change mid-test, capture the SOC-patched state as a SECOND finding (defensive-action observed). Package both via `redteam-report-template`.
### 1. CVE-2021-21972: vCenter `vropspluginui` unauthenticated arbitrary file upload (canonical pre-auth RCE)
- **Affected:** vCenter Server 6.5 < 6.5 U3n, 6.7 < 6.7 U3l, 7.0 < 7.0 U1c (vRealize Operations vCenter plugin bundled with every default install; Linux or Windows variant matters for payload). VMware Cloud Foundation 3.x/4.x also bundles.
### 2. CVE-2021-21985: vCenter vSAN Health Check plug-in pre-auth RCE
### 3. CVE-2021-22005: vCenter Analytics service arbitrary file upload → RCE
### 4. CVE-2022-22954: Workspace ONE Access / Identity Manager FreeMarker SSTI → RCE
### 5. CVE-2022-22972: Workspace ONE Access / Identity Manager / vRealize Automation Host-header authentication bypass
- **Root cause:** Host header used unvalidated as the target for the internal auth-validation request; classic SSRF-into-self with trust elevation.
- **Disclosure:** Reported by Bruno López of Innotec Security; VMware advisory VMSA-2022-0014 published 2022-05-18. **CISA Emergency Directive 22-03** (2022-05-18) ordered all U.S. federal civilian agencies to patch or remove affected VMware installations by 2022-05-24; the same agencies that had just been told the same thing for CVE-2022-22954 six weeks earlier. Year discovered 2022, patched 2022.
### 6. CVE-2023-20887: VMware Aria Operations for Networks (vRealize Network Insight) pre-auth command injection → RCE
- **Attack flow:** Two-issue chain; (a) reach the Apache Thrift endpoint exposed on the management interface despite no authentication, (b) inject shell metacharacters into a parameter passed to a `bash -c` invocation. Single unauth POST → root.
### 7. CVE-2023-34048: vCenter Server DCERPC pre-auth out-of-bounds write → RCE (APT-exploited zero-day)
- **Disclosure:** Reported by Grigory Dorodnov of Trend Micro ZDI (publication ZDI-23-1623, suggesting a paid ZDI submission; bounty undisclosed). Advisory VMSA-2023-23 published 2023-10-24. **Mandiant** later attributed in-the-wild exploitation to **UNC3886** (China-nexus espionage) since late 2021; a ~1.5-year zero-day window before patch. **CISA KEV:** added 2024-01-22. Year discovered 2023 (patched), exploited since 2021.
### 8. CVE-2024-37085: ESXi Active Directory integration "ESX Admins" auth bypass (ransomware-favorite)
- **Disclosure:** Reported by Microsoft Threat Intelligence; advisory VMSA-2024-0013 published 2024-06-25. **Microsoft Security Blog** documents pre-patch exploitation by **Storm-0506 (Black Basta), Storm-1175, Octo Tempest, and Manatee Tempest** ransomware operators; including a confirmed Black Basta deployment at a North American engineering firm. **CISA KEV:** added 2024-07-30. Year discovered 2024 (exploited as 0-day), patched 2024.
### 9. CVE-2024-22273: ESXi / Workstation / Fusion storage controller OOB read/write
- **Note on the original ticket:** This CVE was listed in the brief as "Aria SSRF," but the actual NVD record describes an **ESXi/Workstation/Fusion storage-controller out-of-bounds read/write**, not an Aria SSRF. The closest Aria SSRF-adjacent issue in the 2024 cycle is **CVE-2023-34063** (VMSA-2024-0001); Aria Automation missing access control allowing authenticated cross-org access; CVSSv3 9.9. Both are cited so the matrix is technically accurate.
- **Aria-specific companion; CVE-2023-34063 (VMSA-2024-0001):** Aria Automation 8.11.x–8.14.x missing access control; authenticated actor obtains unauthorized cross-organization access to remote workflows. CVSSv3 9.9. Patched in Aria Automation 8.16. References: https://www.vmware.com/security/advisories/VMSA-2024-0001.html ; https://www.cisa.gov/news-events/alerts/2024/01/17/vmware-releases-security-advisory-aria-automation
### 10. CVE-2020-3992 + CVE-2021-21974: ESXi OpenSLP pre-auth use-after-free / heap overflow (ESXiArgs ransomware vector)
- **Root cause:** OpenSLP daemon; long-deprecated, exposed by default until 2021; has unsafe parsing of SLP message frames.
- **Disclosure:** CVE-2020-3992 reported by Lucas Leong of Trend Micro ZDI (ZDI-20-1376, paid ZDI submission). CVE-2021-21974 reported by Lucas Leong of Trend Micro ZDI as well. **CISA KEV:** CVE-2020-3992 added 2021-11-03; CVE-2021-21974 added 2023-02-08. **ESXiArgs ransomware campaign** (Feb 2023) hit ~3,800 internet-exposed hosts via CVE-2021-21974; two years after patch was available. VMware disabled SLP by default in subsequent releases. Year discovered 2020/2021, patched 2020/2021, mass-exploited 2023.
### Bonus: CVE-2022-31656: Workspace ONE Access / Identity Manager / vRealize Automation auth bypass (post-22972 follow-up)
- **Attack flow:** Network-only actor obtains admin without authenticating; chained with CVE-2022-31659 (auth'd RCE) the pair yields pre-auth admin RCE; the spiritual successor to the 22954+22972 pair.
- **CISA KEV catalog (VMware filter):** https://www.cisa.gov/known-exploited-vulnerabilities-catalog?search_api_fulltext=vmware; VMware is one of the most-represented vendors; cross-check any vCenter/ESXi/Workspace finding here before grading severity.
- **ZDI published advisories:** https://www.zerodayinitiative.com/advisories/published/; filter "VMware" for the canonical paid-bounty disclosures (CVE-2020-3992, CVE-2023-34048, multiple Aria Operations and vRNI).