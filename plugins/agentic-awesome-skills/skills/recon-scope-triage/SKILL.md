2. **Attacking an innocent third party** that merely shares the name; out of scope, and real harm.
| **Mobile apps** | Store search matched the brand word in app name / package | Publisher account = the org; package reverse-DNS = an owned domain (`com.<owneddomain>.app`); dev cert; app calls owned API hosts. Mature ASM tools emit an "apps_accepted=0" / ownership-confidence field; read it. |
| **Typosquats** | Generated permutations of the name | These are *defensive*/brand-protection findings, not offensive scope; note and move on. |
## Web "Critical" triage: the soft-404 control
cmp -s /tmp/a /tmp/b && echo "SOFT-404 false positive" || echo "differs; investigate"
3. **Re-baseline the severity counts** against only-owned assets. Report the *delta*; "N Criticals → M after ownership + soft-404 triage" is itself a finding about the ASM program.
5. **Surface the meta-finding:** if the supplied ASM/recon feed is mostly false-positive, that misallocates the owner's remediation budget and buries real risk; write it up (Medium/Strategic).
- **Counting typosquats / missing-headers / brand-collision repos as offensive findings.** They're defensive/hygiene/noise; they pad the report and erode credibility.
- **Mobile apps** published by entirely different companies that share the name; banks, credit unions, dating apps, dispensaries, home-care services are all real-world collision categories. (Good ASM tooling will tell you it accepted *zero* as owned.)
On a real engagement against a dictionary-word brand, after clearing this noise the only genuinely-owned high-severity finding was discoverable solely by manual tradecraft (a JS-bundle → API discovery, see `hunt-spa-api`); it was nowhere in the hundreds of scanner "Criticals." Triage-first is what made the engagement productive instead of a goose chase.
- **`triage-validation`**; asset-ownership triage (this skill) precedes finding-validity triage (the 7-Question Gate). Ownership first, then validity.
- **`redteam-mindset`**; "aggressive default" means probe every *owned* live surface; this skill defines which surfaces are owned so persistence isn't wasted on collisions.
- **`hunt-spa-api`**; once an API host passes ownership triage, this is how you test it.
- **`offensive-osint` / `osint-methodology`**; feed ownership anchors (verified domains, tenant brand, dev accounts) from OSINT into this triage.