| `actions/*` (GitHub official) | First-party | Hardening candidate; version tags remain mutable |
| `github/*` (GitHub org) | First-party | Hardening candidate; version tags remain mutable |
| External orgs (aws-actions, docker, tj-actions, community, unknown) | Third-party | **Yes**; pin to full SHA when privilege makes it relevant |
- uses: some-org/some-action@v1     # Tag; mutable third-party
- uses: some-org/some-action@main   # Branch; mutable
SHAs are **immutable**; once a commit exists, its SHA cannot change. Pin third-party actions to the full 40-character SHA and add a comment with the version for readability.
First-party tags are mutable too; report them as hardening unless a concrete exploit path warrants a finding. Same-repo or vendored actions are not third-party supply-chain findings (they can still be unsafe if loaded from a PR-controlled checkout; that is a pwn-request / trust-crossing issue, not pinning).
This is not theoretical; CVE-2025-30066 (`tj-actions/changed-files`) and related incidents showed tag rewrites can leak secrets at scale.
# Action's action.yml: RISKY even when SHA-pinned
- **Dependabot**; GitHub native, updates action SHAs
- **Renovate**; Can pin and update actions
- **StepSecurity Secure Workflows**; Can pin actions to SHAs (configure to match third-party-only policy if you do not want first-party pins)
- [GitHub Docs: Security hardening; Using third-party actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions#using-third-party-actions)