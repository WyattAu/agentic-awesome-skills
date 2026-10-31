- Compromising upstream open-source projects (massive blast radius; illegal in most jurisdictions without authorization)
## Step 1: GitHub org discovery
## Step 2: Enumerate public repos for sensitive artifacts
## Step 3: Internal package-name discovery
## Step 4: Dependency-confusion vulnerability check
# Go modules: slightly different, since module names are URLs
## Step 5: Typosquat candidates (around external dependencies)
## Step 6: GitHub Actions workflow injection scan
    # 6. Unpinned third-party actions: mutable tag (@v1, @main) vs pinned (@<40-char sha>)
| `pull_request_target` + `actions/checkout` with `ref: pull_request.head.sha` + uses repo secrets | **Critical**; RCE on runner with org secrets |
| Third-party action pinned to a mutable tag (`uses: org/repo@v1` / `@main`) instead of a commit SHA | **High**; repointable supply-chain vector (see case #9) |
| Self-hosted runner reachable from public repo workflows | **High**; persistent attacker pivot |
## Step 7: Docker / container image registry mining
# GHCR (GitHub Container Registry): public images visible in repo packages tab
## Step 8: SBOM / artifact metadata leakage
## Step 9: Internal registry URL leakage
# Each leaked internal URL is intel: flag the URL itself even if not directly exploitable
## Step 10: npm/PyPI organizational presence
# Check if scope is taken: if it's NOT, an attacker could register
## Step 11: Frontend and third-party dependency checks