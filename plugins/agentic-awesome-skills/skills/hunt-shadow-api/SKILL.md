## OWASP API9: Improper Inventory Management (Shadow / Zombie APIs)
receiving the same security fixes as the current version; because nobody tracks that they
DO NOT use for single-version APIs with no version history; there's nothing to diff; go
## Stage 1: Enumerate the Full Version Surface
## Stage 2: Pull Every Reachable Spec, Not Just the Linked One
# Wayback Machine: a DEPRECATED version's spec often stays indexed after the live link is removed
comm -23 /tmp/v1_paths.txt /tmp/v2_paths.txt   # in v1 only; candidates for "still live but forgotten"
endpoint candidate; carry it into Stage 3.
## Stage 3: Behavioral Diff Between Old and Current Version
  old version accepting what the new one rejects means hardening happened forward-only -
- **Field exposure.** Does the old version's response body include fields; internal IDs, other
  users' data, internal notes, PII; that the current version has since redacted?
## Stage 4: Deprecated / Internal Routes Never Referenced by the Current UI
  `/_internal/`, `/test/`, `/staging/`); reuse `hunt-source-leak`'s JS-bundle grep patterns for
- Check `robots.txt` / `sitemap.xml` for disallowed API paths; a self-inflicted disclosure.
  Informational. The finding is a **security-relevant regression**; auth, rate-limit, or
  claiming a behavioral difference; send a payload that would actually behave differently
  is not a finding; confirm the underlying operation still executes.
- **`hunt-api-misconfig`**; owns exploitation once a spec or endpoint is in hand (mass
- **`hunt-subdomain`**; owns host/subdomain-level discovery (`api-v1.target.com` as its own
- **`hunt-source-leak`**; JS-bundle grep for internal/undocumented calls; reused here
- **`apk-redteam-pipeline`** / **`ios-redteam-pipeline`**; mobile builds routinely hardcode an
- **`hunt-brute-force`**; a rate-limit regression found here is only a complete finding once
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.