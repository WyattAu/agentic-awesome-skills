- Bug bounty programs (use bug-bounty skill; different scope rules)
## Mindset correction #1: The blocker is data, not the stop sign
> "Recheck under load showed no timing differential; recanting the SQLi as indeterminate."
1. **Original PoC artifacts are forever**; capture them BEFORE recheck. Screenshots, request/response pairs, timing samples.
2. **Diff the response**; body size, headers, cookies, response time. The change tells you what the client deployed (WAF rule? Hotfix? Geo block?).
3. **The deployed mitigation is itself a finding**; positive operational observation about IR responsiveness.
4. **Try alternative vectors**; slower-paced timing, encoded keywords, different injection contexts, cookie injection, header injection.
5. **Document both states**; "vulnerable at T0, mitigated at T0+30min, response behavior changed; mitigation layer not established".
## Mindset correction #2: Sister-app pattern recognition
1. **Identify shared infrastructure**; same IP, same load balancer, same TLS cert, same response headers, same session cookie name, same login form HTML.
2. **Identify shared code template**; same form fields, same error messages, same view structure, same framework version.
4. **Document the class of vulnerability**; "vulnerability is in shared form-handler template across N apps", not just one finding.
5. **Recommend class-fix**; fix the shared template, not just one app.
The authorized-engagement case: SQLi confirmed on one sub-app (`<app-A>`); four sibling sub-apps (`<app-B>`, `<app-C>`, `<app-D>`, `<app-E>`) sit on the same employee-app host with similar form patterns; likely all share the same vulnerable template. Should have been a multi-app finding.
## Mindset correction #3: WAPT vs Red Team scope discipline
- IDOR (cross-user read/write *without* an ATO / privilege-escalation / sensitive-data-exfil chain; WAPT class). NOTE: IDOR is NOT a blanket-skip. IDOR on admin/role-assignment endpoints yields privilege escalation; on password-reset or session objects yields ATO; on file-download or PII handlers yields exfil; on token/key endpoints yields credential capture. Pursue those per the decision rule below.
- CSRF (state-change-via-tricked-user; WAPT class)
- Reflected XSS (without account-takeover chain; WAPT class)
**Decision rule:** if the bug, exploited fully, doesn't lead to a session/token/foothold or sensitive data exfil, it's WAPT-class; note it briefly but don't burn time on it.
## Mindset correction #4: Aggressive default, not conservative default
6. **For every "out of scope" SaaS** that's on a corp subdomain, confirm with client; vendor-managed doesn't mean immune (CVE-2022-47966 went unpatched on many on-prem ME-SDP installs)
## Mindset correction #5: Persistence beats elegance
## Mindset correction #6: Real-time IR observation is a finding
- **Document as positive operational finding**; "client SOC detected and mitigated within X minutes; mitigation deployed at WAF/code level"
- **Verify the mitigation depth**; identify the actual layer using evidence, not an assumed WAF/code distinction
- **The original PoC remains the vulnerability finding**; patching doesn't erase it
## Mindset correction #7: Multi-technique cross-validation
## Mindset correction #8: Engagement journal discipline
## Mindset correction #9: Time is the constraint, not skill
- **Don't pre-judge feasibility**; if a dumper would take 6 hours, schedule only explicitly authorized bounded work with a kill switch; do not run unattended extraction.
- **Background long-running jobs**; kick them off, set monitors for events, do other work in parallel.
- **Don't repeat yourself**; if you tested target X with payload Y on Tuesday, Wednesday you should know that without re-testing.
- [ ] If I'm tempted to "stop"; am I sure scope is exhausted, or am I just tired?
- "Recheck failed so it must have been a false positive" (NO; investigate the delta)
- "SaaS, so nothing to test" (vendor patches centrally; usually true, but tenant config gaps are NOT central)
- `m365-entra-attack`; M365 credential attack chain
- `mid-engagement-ir-detection`; turning client SOC patches into findings
- `hunt-sqli`; SQL injection across techniques
- `hunt-rce`; RCE across vectors
- `bug-bounty`; for distinguishing red-team vs bb scope when working dual-track
- **`recon-scope-triage`**; Before the "aggressive default, probe every live surface" directive can be applied safely, you must know which surfaces are actually the target's. Engagement flow: ASM/recon dataset received → `recon-scope-triage` clears namespace-collision noise + soft-404 false positives → only owned, verified assets enter the test queue. Skipping this wastes the engagement on other companies' assets (and risks attacking innocent third parties).
- **`hunt-spa-api`**; Operationalizes the "harvest JS bundles" cadence line into a full play: SPA JS → backend API map → unauthenticated broken-access-control testing. On a real engagement this play (not any scanner) found the apex Critical. Engagement flow: live SPA/`console`/`app`/`api` host identified → `hunt-spa-api` → test each route family unauthenticated against a gated-sibling control.
- **`hunt-dispatch`**; Once mindset is loaded, the `/hunt` command needs a mode answer (redteam vs wapt, blackbox vs greybox) before it routes to platform-specific skills. Engagement flow: red-team mindset triggered → confirm engagement mode (`bug-bounty` vs red-team vs pentest per project memory) → invoke `/hunt` → `hunt-dispatch` loads the right cluster (M365 / SharePoint / VPN / vCenter / APK).
- **`mid-engagement-ir-detection`**; Red-team mindset says "behavior changes ARE findings"; this skill operationalizes that. Engagement flow: red-team engagement underway → baseline established at session start → response patterns shift mid-test → `mid-engagement-ir-detection` captures the SOC-patch state as a NEW finding (defensive-action observed = client capability metric). Don't dismiss it as "the bug got fixed."
- **`redteam-report-template`**; Red-team deliverable is NOT a bug-bounty report; different audience, different tone, different cadence. Engagement flow: findings collected throughout engagement → at session close, package via `redteam-report-template` (Subject / Observations / Description / Impact / Recommendation / PoC) for client-facing DOCX, not `report-writing` which is for H1/Bugcrowd/Intigriti platforms.
- **`triage-validation`**; Red-team mindset includes "don't retract too fast"; the 4 retractions from an authorized engagement were mindset failures, not validation failures. Engagement flow: every finding through `triage-validation` 7-Question Gate, but with the red-team adjustment that "exploitable only with chain" is still a finding, not a no-finding.
- **`evidence-hygiene`**; Red-team engagements often span weeks; without disciplined evidence capture the deliverable suffers. Engagement flow: red-team mindset triggered → set up `evidence-hygiene` capture cadence (screenshots, request/response dumps, timestamped logs) at session start, not at session close.