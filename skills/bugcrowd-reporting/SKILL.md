# BUGCROWD REPORTING: Program-Specific Tactics
## 1. VRT Category Selection: Search & Fallback Strategy
Bugcrowd's submission form requires a single VRT (Vulnerability Rating Taxonomy) selection. The dropdown's default severity is bound to the chosen node; pick wrong and the form auto-suggests a lower priority (often P4) when the actual impact is P3 or P2.
> Note: VRT default severities are not fixed constants. Bugcrowd revises the VRT schema across versions, and individual programs can remap defaults via their own priority configuration. The P-values shown in the examples below (e.g., "No Rate Limiting on Form → Login" defaulting to P4) are the typical baseline at time of writing; always read the severity the *current* form actually auto-suggests for *this* program rather than assuming the value here.
1. **The bug's primary class**; e.g., `IDOR`, `XSS`, `SSRF`, `auth bypass`, `2FA bypass`
2. **The data category exposed**; e.g., `PII`, `sensitive data exposure`, `disclosure of secrets`
3. **The control bypassed**; e.g., `broken access control`, `authentication bypass`
4. **The endpoint type**; e.g., `no rate limiting on form > login`, `no rate limiting on form > change password`
5. **The generic parent node**; e.g., `Server Security Misconfiguration > Other`, `Broken Access Control > Other`
## 3. Severity-Request Paragraph: Always First in the Body
## Severity request: please review carefully before applying VRT default
1. **[Impact axis 1]**; [specific reason this finding exceeds the VRT default's example]
2. **[Impact axis 2]**; [specific reason, often citing the program's own Focus Areas]
3. **[Impact axis 3]**; [specific reason, often comparing to historical bounty payouts for the same data class]
## Severity request: please review carefully before applying VRT default
1. **[Endpoint] is a password-disclosure oracle, not a generic rate-limit gap.** The endpoint returns true/false for a supplied password; combined with cookie theft, it is the exact primitive an attacker uses to learn the victim's *durable* credential from a stolen *transient* session.
The "P4 = No Rate Limiting on Login Form" default applies to ordinary login pages where brute-force is bounded by lockout policies. This endpoint has neither lockout nor throttle, and the result of brute force is the *user's actual password* (not a session); making the impact significantly higher than the default VRT severity assumes.
- Open with *"Severity request; please review carefully before applying VRT default"* (literal phrase). This wording has no guaranteed effect on triage.
## 4. OOS-Clause Rebuttals: Templates
The program OOS list excludes "Rate limiting or brute-force issues on **non-authentication** endpoints." `[endpoint name]` is the canonical authentication endpoint; it accepts a password/token/OTP and returns whether it matches the user's stored credential. By definition this is an authentication primitive. The OOS clause's "non-authentication" qualifier therefore does NOT apply.
This is not a "debug information" finding. The [persisted-query allowlist | introspection gate | etc.] is acting as an authorization control on the [GraphQL surface | API tree | etc.]. The bypass converts that gate into a no-op; multiple [mutations | endpoints] become reachable that the official clients cannot invoke. The schema disclosure is incidental; the impact is **mutation-surface unlock**, not information leakage.
## 5. Chained Findings: Cross-Reference Strategy
- **submission [UUID-1]**; [primitive 1: e.g., `me.verify_password` no rate limit]
- **submission [UUID-2]**; [primitive 2: e.g., GraphQL APQ allowlist bypass]
- Don't claim each primitive is independently P1; overclaiming damages credibility for all linked reports.
## 6. Target Selection: QA vs. Production
- Production may enforce additional controls that QA does not; please confirm whether the same resolver code path runs on production.
### 7.1 Account email: Bugcrowdninja alias
2. Document the restoration in the report; *"Account state was restored immediately after the PoC."*
3. Optionally, rotate the test-account password to a fresh value not used elsewhere; that way passwords visible in PoC screenshots are also dead
1. Don't create a second account to bypass the lock; that looks like fraud
This skill is small and focused. It does NOT duplicate content from `report-writing` (per-platform templates, CVSS scoring, downgrade counters, 60-second pre-submit checklist); load both skills together when filing a Bugcrowd report. The two skills' content composes naturally because their boundaries are clean: `report-writing` is the body templates, `bugcrowd-reporting` is the program-specific tactics layered on top.
- **`report-writing`**; Always load this skill alongside `bugcrowd-reporting`. Workflow primitive: `report-writing` provides the body skeleton; this skill layers VRT selection + severity-request paragraph + OOS-clause rebuttals on top of that skeleton.
- **`triage-validation`**; When deciding if a Bugcrowd-bound finding will pass triage. Workflow primitive: 7Q gate runs first; only validated findings reach this skill for VRT mapping.
- **`evidence-hygiene`**; When attaching screenshots / HARs to a Bugcrowd submission. Workflow primitive: Bugcrowd's private attachment system requires redacted evidence; route everything through `evidence-hygiene` before clicking attach.
- **`security-arsenal`**; When the PoC step needs payloads cited verbatim. Workflow primitive: the "Steps to Reproduce" section in this skill's report bodies pulls exact payloads from `security-arsenal` so the triager can paste-and-run.
- **`bb-methodology`**; When confirming engagement mode is bug-bounty (not red-team). Workflow primitive: PART 0 of `bb-methodology` answers "bug bounty?"; if yes AND the target is Bugcrowd, this skill becomes the reporting overlay.
- Process guidance only; it does not authorize probing or exploitation; those steps need their own authorized skill and confirmation gate.