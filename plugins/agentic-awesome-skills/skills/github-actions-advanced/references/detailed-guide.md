permissions:                 # Always declare; principle of least privilege
    runs-on: ubuntu-24.04    # Pin OS version; never use -latest in prod
    timeout-minutes: 15      # Always set; prevents runaway jobs
  pull_request_target:                   # Runs with repo secrets; use with care!
### Language Setup Actions (Preferred: No Extra Step Needed)
- **Required reviewers**; manual approval gate before run
- **Wait timer**; delay after approval (e.g., 10-minute buffer)
- **Branch/tag restrictions**; only `main` or `v*` tags can deploy to prod
- **Environment-specific secrets**; override repo-level secrets per environment
- **Deployment branches**; whitelist which branches can target this environment
# Auto-provided token: no setup needed
# Actions Runner Controller (Kubernetes): recommended for ephemeral runners
- `gha-security-review`; Deep security audit of existing workflow files
- `github-actions-templates`; Copy-paste ready workflow templates
- `docker-expert`; Container build optimization and Dockerfile best practices
- `kubernetes-architect`; Deploying to Kubernetes from GitHub Actions
- `gitlab-ci-patterns`; GitLab CI/CD equivalent patterns