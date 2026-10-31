- iOS-only targets (different pipeline; IPA reverse, MobSF, frida-ios-dump)
## Stage 0: Inventory all org-owned apps
Example output (anonymized; 7 packages typical for a multi-brand conglomerate):
## Stage 1: APK acquisition
## Stage 2: DEX decompilation (jadx)
## Stage 3: Candidate secret patterns
# URL grep: owned-domain references
# Hardcoded passwords (heuristic: many false positives, manual review)
### Real-world example finding (anonymized: from an authorized engagement)
# Expired ~8 years earlier: but path tokens + 30 /v1/* endpoints still useful intel
## Stage 4: Pinned certificate extraction
A customer-facing APK from an authorized engagement contained `assets/api_<service>_<domain>_com.cer`; revealed the existence of an `api.<service>.<domain>.example` asset that had NOT surfaced in passive recon.
## Stage 5: Exported component enumeration
AndroidManifest.xml lists components. Exported ones (especially with `android:exported="true"` or implicit-export via intent-filter) can be triggered by other apps; potential intent-injection attack surface.
## Stage 6: Firebase / cloud-service config inspection
## Stage 7: Runtime instrumentation (Frida)
For when static analysis isn't enough; you want to dump tokens at runtime, bypass cert pinning, or trace API calls.
## Stage 8: Network traffic capture (via mitmproxy)
## Decision tree: what to do with what you find
| Expired JWT | Inspect path tokens / API endpoint structure; useful intel for post-VPN |
| AWS access key | Use `awscli` to test: `aws sts get-caller-identity`; many leaked keys still have permissions |
| Google API key (AIza*) | Test against `https://www.googleapis.com/customsearch/v1` etc.; see what API the key activates |
- **Don't grep only for "password"**; most secrets have specific high-signal patterns (AKIA, AIza, eyJ, etc.). Generic word grep produces too much noise.
- **Don't skip XAPK split APKs**; config.armeabi splits and config.<lang> splits sometimes contain different code paths.
- **Don't trust expired JWTs as "dead intel"**; the path structure, endpoint list, and signing algorithm are still useful. The 8-year-expired-JWT example above shows this.
- **Don't reverse only the latest version**; older APK versions (via APKMirror version history) sometimes have secrets removed in newer versions but still active server-side.
- **Don't ignore Firebase even if app looks "simple"**; Firebase rules misconfigurations (public read on Firestore) are extremely common.
- **Don't run Frida on a production device**; use rooted emulator or test device only.
- `offensive-osint`; Play Store / APKMirror enum is part of broader OSINT
- `hunt-cloud-misconfig`; Firebase public-read is exactly this skill's specialty post-discovery
- `hunt-api-misconfig`; JWT manipulation once you have the algorithm + sample token
- `evidence-hygiene`; extracted secrets need redaction before report inclusion
- `redteam-mindset`; APK reverse on day-1 of an engagement, not as an afterthought
## Real-world finding template (anonymized: authorized engagement)
- **`cloud-iam-deep`**; APK secret extraction frequently yields live AWS/GCP/Azure credentials. Chain primitive: jadx string-grep produces AWS Access Key ID + Secret → `cloud-iam-deep` `aws sts get-caller-identity` → role/policy enumeration → IAM privilege-escalation path (one of 24 documented AWS escalation patterns) → cloud-plane takeover. Same flow applies to GCP service-account JSON and Azure shared-access-signature tokens extracted from APK resources.
- **`hunt-api-misconfig`**; APK endpoint inventory hands you the API surface for free; mass-assignment, JWT, and CORS bugs are typical. Chain primitive: APK reveals `/v1/users/me` and `/v1/admin/users` → `hunt-api-misconfig` mass-assignment probes (`{is_admin:true}`) against `/v1/users/me` → admin role escalation → access to `/v1/admin/users`.
- **`hunt-rce`**; Hardcoded JWT signing secrets (HS256) extracted from APK enable arbitrary token forging. Chain primitive: APK strings yield HS256 secret → forge admin token → access admin API → if API has eval/template/sink → `hunt-rce` to server. Also: exported components with intent-injection sinks can reach `Runtime.exec` if app is local-installed.
- **`offensive-osint`**; APK is one node in the broader org recon graph; pair with breach corpora and cert transparency. Chain primitive: APK reveals internal API hostname `api2.example.com` → `offensive-osint` certificate-transparency lookup → discover sibling subdomains → expanded attack surface.
- **`redteam-report-template`**; APK findings need clear "what the binary leaks" framing because client engineers often dismiss mobile-static findings as "obfuscation problem." Chain primitive: validated finding (token/URL/secret extracted) → `triage-validation` 7-Question Gate (specifically: "does this credential still authenticate today?") → `redteam-report-template` packaging with explicit binary version + extraction reproduction steps.