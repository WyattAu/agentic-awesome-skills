Find exploitable vulnerabilities in GitHub Actions workflows. Every finding MUST include a concrete exploitation scenario; if you cannot trace an attack path, do not report it as confirmed. Use harmless PoC sketches only; do not trigger workflows, exfiltrate secrets or post attacker content without explicit test authorization.
This skill encodes attack patterns from real GitHub Actions exploits; not generic CI/CD theory.
- `.github/workflows/*.yml`; all workflow definitions
- `action.yml` / `action.yaml`; composite actions in the repo
- `.github/actions/*/action.yml`; local reusable actions
Only report vulnerabilities exploitable by an **external attacker**; someone **without** write access to the repository. The attacker can open PRs from forks, create issues, and post comments. They cannot push to branches, trigger `workflow_dispatch`, or trigger manual workflows.
- `workflow_dispatch` input injection; requires write access to trigger
1. **Entry point**; How does the attacker get in? (fork PR, issue comment, branch name, etc.)
2. **Payload**; What does the attacker send? (actual code/YAML/input)
3. **Execution mechanism**; How does the payload run? (expression expansion, checkout + script, etc.)
4. **Impact**; What does the attacker gain? (token theft, code execution, repo write access)
5. **PoC sketch**; Concrete steps an attacker would follow
Load references selectively; only what's relevant to the triggers found.
- Confirm the value is attacker-controlled (PR title, branch name, comment body; not numeric IDs, SHAs, or repository names)
| `${{ github.event.pull_request.number }}` in `run:` | Numeric only; not injectable |
| Any expression in `workflow_dispatch`/`schedule`/`push` to protected branches | Requires write access; outside threat model |
1. **Read the full workflow**; don't rely on grep output alone
2. **Trace the trigger**; confirm the event and check `if:` conditions that gate execution
3. **Trace the expression/checkout**; confirm it's in a `run:` block or actually references fork code
4. **Confirm attacker control**; verify the value maps to something an external attacker sets
5. **Check existing mitigations**; env var wrapping, author_association checks, restricted permissions, SHA pinning
- **Confidence**: HIGH; confirmed through attack path tracing