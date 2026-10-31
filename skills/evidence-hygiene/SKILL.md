# EVIDENCE HYGIENE: PoC Capture & Redaction Discipline
The core principle: **Bug-bounty evidence is meant to convince a triager. Anything beyond that; live cookies, real-user PII, internal trace IDs that aren't useful; should not be in the evidence.**
| **Your-account secrets** | Session cookies, OAuth tokens, refresh tokens, API keys | Always redact. Even in private bug-bounty platform attachments. Your account, your session; protect it. |
| **Triager-useful metadata** | Trace IDs (`x-datadog-trace-id`), request IDs, server timestamps, your test account UID/email, GraphQL operation names, response shapes | **Review and minimize**; selected nonsecret values help the triager correlate to logs and reproduce. |
- The session cookie (`authn`, `session`, `sid`, `__Secure-id`, etc.; name varies per target)
**Method A; Don't capture the cookies in the first place** (preferred when possible)
**Method B; Destructive redaction on a flattened copy** (when capture includes cookies)
**Method C; Find/replace in raw text** (for HAR files, terminal transcripts)
[ ] Search for the session cookie name substring in any visible text; if present, redact
[ ] Search for the literal first 6 chars of your cookie value; if present, redact
[ ] Compare to the previous PoC screenshot in the same engagement; same redaction discipline
- Government IDs (SSN, passport; mask everything)
- Your own (attacker session's) UID / email; this proves cross-account
### 3.3 Worked example: IDOR PoC body
{"data":{"contact":{"first_name":"<REDACTED; real first name>","last_name":"<REDACTED; real last name>","email":"<REDACTED>@example.com","phone":"<REDACTED>"}}}
1. In the Repeater request panel, the Cookie header is on its own line; drag the panel divider DOWN to hide everything below the request line / target line
3. Don't include the Request / Response sub-panels; they contain the cookie
Almost never the right screenshot; it shows entire request/response pairs with cookies. Use Repeater for demos instead.
The Scanner tab's Issues panel may contain sensitive URLs/evidence; inspect it before screenshotting; it shows finding summaries without the underlying request bodies. Click into a specific finding before screenshotting only if you've redacted its evidence first.
  credentials: 'include',  // sends cookies automatically; they won't appear in your code
Take the screenshot immediately after the response prints; don't wait for unrelated framework warnings to appear.
    // (Don't dump the full j; your screenshot becomes unreadable)
1. **Pre-state verification**; `verify_password("current") → true`. Proves you knew the starting state.
2. **The bug itself**; `update_password(...)` succeeds without step-up. THE most important screenshot.
3. **Post-state verification (negative)**; `verify_password("old") → false`. Proves the change took effect.
4. **Post-state verification (positive)**; `verify_password("new") → true`. Proves the new credential is active.
5. **Out-of-band side effects**; inbox screenshot showing whether a notification email arrived. Proves whether passive defense exists.
- **Screenshot 1 (`04-step1-verify-true-before.png`)**; verify_password returns true with the current password.
- **Screenshot 2 (`04-step2-update-password-no-stepup.png`)**; update_password returns User with no StepUpRequiredError. THIS IS THE BUG.
- **Screenshot 3 (`04-step3-verify-false-after.png`)**; verify_password with old password returns false.
- **Screenshot 4 (`04-step4-verify-true-new-pw.png`)**; verify_password with new password returns true.
- **Screenshot 5 (`04-step5-inbox-notification-check.png`)**; inbox showing whether out-of-band notification was sent.
Keep your unredacted HARs / screenshots in a local folder accessible only to you. The triager may ask for the unredacted version during verification. **Never share unredacted artifacts via email; use the platform's private attachment system on the existing submission thread.**
Quarterly, sweep your `~/security-research/` and `~/Downloads/` for stale HARs / screenshots from old engagements. Either move them to a properly-encrypted archive or delete them. Bug-bounty engagement artifacts are sensitive; they accumulate fast and become a liability.
- **What** to capture (that's `triage-validation` and `report-writing`'s job; they tell you what evidence is required for the report to be accepted)
- **How** to demonstrate exploitation (that's `security-arsenal`'s job; payloads to send)
- **Where** to find the assets (that's `offensive-osint`'s job; recon)
- **`report-writing`**; When evidence is redacted and ready for the report body. Workflow primitive: after redaction passes the pre-screenshot checklist, hand off to `report-writing` for the platform-specific report template (H1 / Bugcrowd / Intigriti / Immunefi).
- **`bugcrowd-reporting`**; When the redacted evidence is destined for Bugcrowd specifically. Workflow primitive: redacted screenshots + sanitized HARs become attachments referenced in `bugcrowd-reporting`'s VRT-mapped submission body.
- **`triage-validation`**; When deciding which evidence is required at all. Workflow primitive: `triage-validation` says "you need a 5-screenshot PoC for password-change-without-step-up"; this skill says "here's how to capture and redact those 5 screenshots cleanly."
- **`security-arsenal`**; When the payload that produced the evidence is being documented. Workflow primitive: the payload (from `security-arsenal`) goes in the report verbatim; its output / response (handled by this skill) gets cookie / PII redacted before screenshotting.
- **`bb-methodology`**; When Phase 5 (Validate & Report) needs evidence captured. Workflow primitive: Phase 5's evidence-capture step routes through this skill before the report is drafted.
- Process guidance only; it does not authorize probing or exploitation; those steps need their own authorized skill and confirmation gate.