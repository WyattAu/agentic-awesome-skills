# VULNERABLE: No explicit permissions: inherits repo default
| `id-token` |; | Request OIDC token | Cloud authentication |
# VULNERABLE: Subject claim too broad: any branch can assume role
| `repo:org/repo:*` | **High** | Never; any branch/PR can assume role |
- [GitHub Blog: Security hardening; Permissions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)