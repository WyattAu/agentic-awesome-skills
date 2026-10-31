# HUNT-SOURCE-LEAK: Source Code & Build Artifact Leakage
- **`.js.map` source maps**; reconstruct full TypeScript/ES6 source code → find hardcoded API keys, internal endpoints, auth logic bypasses
- **`swagger.json` / `openapi.json`**; complete REST API specification with all endpoints, parameters, auth schemes, and internal route names
- **`.env` / `.env.production`**; APP_KEY, DB_PASSWORD, API_KEY, SECRET_KEY in plaintext
- **`.git/` exposure**; `git clone` the entire source history → all past hardcoded secrets
- **`asset-manifest.json` / `_next/static/`**; all JS bundle paths → systematic source map discovery
- **`build-info` / `info.json`**; git commit hash, build timestamp, dependency versions → CVE targeting
## Phase 1: Quick Wins (Run First)
## Phase 2: Source Map Discovery
> fully exposed under a new name. **A 404 at the old URL is not remediation** -
> `main.<hashB>.js`; and the map was still published at `main.<hashB>.js.map`,
## Phase 3: Swagger / OpenAPI Discovery
## Phase 4: .git Exposure
## Phase 5: Forgotten Files & Debug Endpoints
## Phase 6: .DS_Store File Listing
## Phase 7: webpack Chunk Analysis
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.