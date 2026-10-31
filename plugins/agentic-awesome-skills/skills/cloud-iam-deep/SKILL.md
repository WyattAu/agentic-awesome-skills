- Post-RCE on a cloud-hosted instance; pivot to cloud control plane
- On-prem-only environments (use AD attack skills; but those are out of scope per external-only boundary)
- Web2 vulns that happen to be on AWS; use the relevant `hunt-*` skill
# AWS secret pattern (40-char base64-ish: context required)
# K8s SA token (JWT format: decode to confirm)
## AWS: read-only validation (the safe first step)
# Try common read-only first: failures still inform you
## AWS privesc patterns (24+ documented: `iam_privesc` techniques)
Quick lookup; if you have any of these IAM actions, escalate via the listed technique:
## AWS: STS / cross-account / role chaining
# IMDSv1 (legacy, still common: straight GET):
# IMDSv2 (requires PUT to get a token first: usually mitigates SSRF):
## Azure: credential validation
## Azure: Managed Identity abuse
## GCP: service account JSON
# What roles does this SA have? (project-level only: not org-level)
## Kubernetes: exposed API / SA token
# Anonymous binding (system:anonymous user): surprisingly common
- **DO NOT run write/delete operations without explicit OK**; IAM mutation is destructive and audit-visible
- **DO NOT enumerate everything in scope of an account**; `aws iam list-users` against an account with 50,000 users is loud and slow
- **DO NOT use `aws *` with non-test creds without confirming you have the right account**; accidentally hitting prod = career risk
- **DO NOT confuse "I have the credential" with "this credential is current"**; always check expiration / rotation via STS first
- **DO NOT assume an STS token from one account works across accounts**; region restrictions and trust policies apply
- **DO NOT skip CloudTrail/Activity Log awareness**; logging coverage depends on service/event configuration and retention; pair with `mid-engagement-ir-detection`
- **DO NOT pivot deeper than the SOW allows**; discovering admin creds doesn't mean using them; some engagements are read-only
- `hunt-cloud-misconfig`; finds the credentials in the first place (public buckets, IMDS via SSRF, leaked JSON)
- `hunt-ssrf`; SSRF→IMDS is the canonical chain into cloud control plane
- `apk-redteam-pipeline`; APK secret extraction commonly yields cloud creds
- `supply-chain-attack-recon`; CI/CD pipelines store cloud creds; finding them is a separate workflow
- `m365-entra-attack`; Azure cross-product; Managed Identity tokens cross over to Graph
- `mid-engagement-ir-detection`; cloud control plane activity is monitored; expect mitigations
| Leaked cred; read access only to non-sensitive | Medium |
| Anonymous public bucket; listing only | Low/Medium |
| Anonymous bucket; write permission | High |
- Used `sts:AssumeRole` to chain; note the role names and times in IoCs
- Created any IAM resources (test users, roles, policies); list them with explicit cleanup confirmation
- Read sensitive data (Secrets Manager, KMS keys, Storage blob content); note in deliverable that data was viewed but not exfiltrated outside the engagement systems
- [AWS Cognito Identity Pool; Unauthenticated-Role Attack Chain (2024-2026 surface)](references/details.md)