- **SOC 2 preparation**; auditors need evidence of MFA, access controls, and OAuth governance.
- **Suspicious OAuth app**; an employee authorized a third-party app with broad scopes.
- **SaaS sprawl**; teams sign up for tools with company email and nobody tracks them.
- **Post-incident hardening**; after phishing or credential leaks, tighten every surface.
### Google Workspace: OAuth Grants
### GitHub: Installed Apps
### Slack: Approved and Pending Apps
### AWS: IAM Credential Report
# Root account lockdown: verify MFA, remove access keys
# Google: find apps with dangerous scopes
# GitHub: find apps with write access
CRITICAL; revoke unless justified:
HIGH; review carefully:
LOW; generally safe:
# Google Drive: block external sharing and restrict viewers
# GitHub: enable secret scanning and push protection org-wide
# AWS: block all public S3 access at account level
  [ "${count}" -gt 0 ] && echo "DETECTED: ${domain}; ${count} queries"
## Quick Reference: Top 10 Priority Actions