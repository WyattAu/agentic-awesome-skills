- `apk-redteam-pipeline` already found Android endpoints/secrets; the iOS build often ships a *different* backend version worth diffing (see `hunt-shadow-api`)
- Android-only targets; that's `apk-redteam-pipeline`
## Stage 0: Inventory all org-owned iOS apps
Extract: `trackId`, `bundleId`, `sellerName` (developer account; pivot to find sibling apps), `version`,
`releaseNotes` (changelogs often reference deprecated/removed API behavior; feeds `hunt-shadow-api`).
## Stage 1: IPA acquisition
staging API hosts hardcoded); compare channel/build/version; a beta finding does not establish production exposure.
# Look for <key>software-package</key>: that URL is a directly downloadable, unencrypted IPA
Enterprise/ad-hoc IPAs are **not FairPlay-encrypted**; no jailbreak or decryption tooling needed,
python3 dump.py <bundle_id>          # frida-ios-dump; outputs a decrypted .ipa
## Stage 2: Unpack and static analysis
# Info.plist: bundle ID, URL schemes, ATS config, entitlements hint
# Entitlements: codesign reads the .app bundle (or the Mach-O binary), NOT Info.plist
# Swift binaries: class-dump won't show much: use Hopper, Ghidra, or `nm`/`strings` instead
## Stage 3: Secret grep (same catalog as apk-redteam-pipeline, iOS-specific sources added)
# URL grep: owned-domain references
# Cloud credentials (same 60-pattern catalog as Android: reuse verbatim)
# Look for API_KEY, PROJECT_ID, STORAGE_BUCKET, GCM_SENDER_ID, GOOGLE_APP_ID: same Firebase
# App Transport Security exceptions (Info.plist): tells you if arbitrary/insecure loads are allowed
## Stage 4: App Transport Security (ATS) misconfig + certificate-pinning bypass
- `NSAllowsArbitraryLoads = true` at the top level; HTTP (cleartext) allowed anywhere, app-wide.
- `NSAllowsArbitraryLoadsInWebContent = true`; WKWebView traffic exempted, common blind spot.
If you want a Frida script instead, don't hand-roll one; use a maintained universal bypass
`SSL_get_psk_identity` at the native layer and forces the verify callback to return "OK"; this
**objection's `ios sslpinning disable`** over any hand-rolled script; both cover the common
## Stage 5: URL scheme / Universal Link enumeration
# Custom URL schemes (CFBundleURLTypes): anything can invoke these via Safari/another app
# Universal Links / Associated Domains (applinks:): requires a matching apple-app-site-association
  `myapp://reset-password?token=x&redirect=https://evil.com`; does the app trust an
  app can intercept traffic intended for the legitimate app; check `CFBundleURLName` uniqueness.
## Stage 6: Runtime instrumentation (Frida / objection)
# host frida-tools version exactly: version skew is the #1 cause of "failed to attach" errors.
# Certificate Trust Settings: iOS requires this second step, unlike Android)
## Decision tree: what to do with what you find
| `NSAllowsArbitraryLoads = true` | Confirm a token/credential actually transits HTTP; needs a network position to exploit, but is a real finding on its own (config-level, no MITM needed to report) |
| Hardcoded JWT / API key in strings dump | Same as Android; test against the API host, chain into `hunt-api-misconfig` |
| Entitlements show `keychain-access-groups` shared with other apps | Cross-app Keychain read; a compromised sibling app reads this app's secrets |
- **Don't assume App Store = FairPlay-encrypted requires jailbreak**; TestFlight and enterprise/ad-hoc
- **Don't skip Swift binaries because class-dump shows nothing**; `strings` and `nm` still work; a
- **Don't stop at "pinning is present" as a finding**; pinning is a mitigation, not a vulnerability;
- **Don't ignore WKWebView-loaded content**; `NSAllowsArbitraryLoadsInWebContent` is a distinct,
- **Don't run Frida instrumentation against a personal/production Apple ID device**; use a
- **`apk-redteam-pipeline`**; the Android counterpart; run both when a target ships on both
- **`hunt-shadow-api`**; mobile builds (iOS and Android alike) often hardcode an older API
- **`cloud-iam-deep`**; IPA secret extraction frequently yields live AWS/GCP/Azure credentials
- **`hunt-api-misconfig`**; hardcoded JWTs/API keys extracted here feed directly into JWT
- **`evidence-hygiene`**; extracted Keychain items and secrets need redaction before report
- **`offensive-osint`**; App Store developer-page enumeration is part of the broader org recon