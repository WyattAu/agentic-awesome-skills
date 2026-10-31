Stored XSS in profile bio field executes in admin panel; allows privilege escalation
**CVSS 3.1 Score:** 6.5 (Medium); AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N
[Same structured steps; exact HTTP requests, exact responses]
P2 (High); Direct read access to other users' PII. Affects all user accounts.
CVSS 3.1 Score: X.X ([Severity]); AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N
[Screenshot or Loom video showing the impact; Intigriti triagers prefer video for complex bugs]
- PoC video is valued much more than screenshot alone; record with Loom
# [Bug Class]: [Protocol Name]: [Severity]
// Foundry PoC; run: forge test --match-test test_exploit -vvvv
| "Requires authentication" | "Attacker needs only a free account; no special role or permission" |
| "Already known" | "Show me the report number; I searched hacktivity and found none" |
| "Low CVSS" | "CVSS doesn't capture business impact; attacker can extract [X] in [Y] minutes" |
[ ] Two test accounts used; not just one account testing itself
[ ] Report is < 600 words; triagers skim long reports
[ ] Severity claimed matches impact described; don't overclaim
- Process guidance only; it does not authorize probing or exploitation; those steps need their own authorized skill and confirmation gate.