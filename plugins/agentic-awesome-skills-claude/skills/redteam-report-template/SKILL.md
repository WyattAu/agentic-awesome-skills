<Bulleted list of what was observed during testing. Concrete facts only; no interpretation yet.>
<What an attacker could achieve. Concrete attacker outcomes, NOT generic CIA triad statements. Tie to the client's business; money, data, reputation, regulatory exposure.>
<Specific, actionable remediation. Vendor patch, configuration change, code-level fix. Avoid "implement security best practices"; say what specifically.>
### Severity table (client-facing: different from CVSS-only)
- **Confirmed**; reproduced multiple times, with full PoC
- **Confirmed; patched mid-engagement**; was reproducible, client patched during the test window (still ship the finding; see `mid-engagement-ir-detection`)
- **Confirmed; partially reproducible**; works but needs specific conditions
- **Suspected (1 signal)**; single indicator, not confirmed (rare; usually drop)
- **Out-of-band**; finding from passive recon, not actively tested
4. Findings (one per ## section, in severity order; Critical first)
   - Credentials in breach corpora (count + sample only; redact)
## Writing tone: for client deliverables
- "Could potentially"; prove it or drop it
- "It might be possible"; same
- "We recommend implementing security best practices"; say which one specifically
## Audience translation: same finding, different framing
The same finding's Impact paragraph should cover both ends; start with the business outcome, then drop into technical detail.
Red-team deliverables should include; not just bug-bounty payable bugs:
- **Information disclosure that helps attack mapping** (CodeIgniter debug toolbar leaking routes, version banners on appliances); Medium
- **User enumeration** (Microsoft OneDrive 302 vs 404 differential); Medium
- **Pre-existing security state observations** (247 accounts already locked by external attacker; weak password policies); Informational with stakeholder relevance
- **Defensive observations** (SOC patched our SQLi within 30 min; evidence of working detection); Informational/positive
- **Sister-app pattern issues** (same code template across 7 apps); Medium (multiplied blast radius)
Bug bounty would reject most of these. Red-team deliverables embrace them; the client paid for the assessment to know.
- Tooling failures (e.g., MCP timeout, CAPTCHA not solvable); these affect what was/wasn't testable
- [ ] DOCX render check; image count matches MD reference count
- `report-writing`; bug-bounty platform reports (different format, different audience)
- `redteam-mindset`; informs what counts as a finding worth shipping
- `mid-engagement-ir-detection`; informs the "patched mid-engagement" status pattern
- `evidence-hygiene`; informs screenshot redaction discipline
- `m365-entra-attack`, `enterprise-vpn-attack`, etc.; each provides finding-templates specific to its attack surface
- **DO NOT** write a 50-page report for 3 findings; pad-by-page erodes credibility
- **DO NOT** use the same severity for everything; calibrate
- **DO NOT** include findings without PoCs; they read as speculative
- **DO NOT** skip the Recommendation section's specificity; "patch and review" doesn't help
- **DO NOT** mix bug-bounty CVSS scoring with red-team severity unthinkingly; context differs (e.g., a Medium on a CVSS basis can be Critical for the client if it touches a regulated dataset)
- **`triage-validation`**; This template ingests findings that have ALREADY passed the 7-Question Gate. Engagement flow: every finding through `triage-validation` first → only validated findings → `redteam-report-template` packaging. Skipping triage produces a deliverable padded with informational noise that erodes client trust.
- **`evidence-hygiene`**; The DOCX with 16 embedded screenshots only works if evidence was captured systematically throughout the engagement. Engagement flow: `evidence-hygiene` discipline at session start → timestamped, organized screenshot folder → `redteam-report-template` consumes that folder to populate Evidence blocks.
- **`redteam-mindset`**; The Subject / Observations / Description / Impact / Recommendation / PoC structure assumes the operator already thinks like a red-teamer (impact-first, blast-radius framing). Engagement flow: `redteam-mindset` loaded at engagement start → findings collected with red-team framing baked in → `redteam-report-template` produces deliverable without rewriting every Impact section.
- **`mid-engagement-ir-detection`**; Defensive-action findings (SOC patches mid-test, new IPS rules deployed, account lockouts triggered by external attacker) are first-class findings in red-team deliverables. Engagement flow: `mid-engagement-ir-detection` captures behavior-change events → each becomes its own Subject in the deliverable, framed as "client capability observation" not as "bug we missed."
- **`report-writing`** + **`bugcrowd-reporting`**; Bug-bounty platform reports use DIFFERENT structure (one finding per submission, platform-specific severity scoring, OOS-clause counters). Engagement flow: if engagement mode is `bug-bounty` per project memory → use `report-writing` / `bugcrowd-reporting` instead. This template is ONLY for external red-team / enterprise client deliverables.
- Process guidance only; it does not authorize probing or exploitation; those steps need their own authorized skill and confirmation gate.