# HUNT-OPEN-REDIRECT: Open Redirect
- **Open redirect → OAuth auth code theft**; redirect_uri contains open redirect on trusted domain → auth code sent to attacker → ATO
- **Open redirect → phishing**; users trust the URL because it starts with target.com
- **Open redirect → SSRF escalation**; if redirect followed server-side → SSRF
- **Open redirect → session fixation**; force user to login endpoint with pre-set session
### Phase 1: Discover Redirect Parameters
### Phase 2: Basic Test
### Phase 3: Bypass Techniques
### Phase 3b: DOM-based open redirect (client-side sink)
### Phase 4: OAuth Chain Test
### Phase 5: Server-Side Redirect (SSRF escalation)
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.