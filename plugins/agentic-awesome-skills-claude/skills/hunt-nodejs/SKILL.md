# HUNT-NODEJS: Node.js Specific Vulnerabilities
- **Prototype Pollution → RCE**; `__proto__` injection via `lodash.merge` / `Object.assign` → polluted prototype reaches `child_process.exec` or `vm.runInNewContext` sink
- **Express trust proxy**; `app.set('trust proxy', true)` without validation → attacker sets `X-Forwarded-For` to bypass IP allowlists or rate limits
- **EJS/Pug SSTI**; template engine receives user input → `{{= process.mainModule.require('child_process').execSync('id') }}`
- **`child_process` injection**; user input interpolated into shell command string → OS command injection
- **`require()` path traversal**; attacker-controlled module path → load arbitrary file as JS
## Phase 1: Fingerprint
## Phase 2: Prototype Pollution Detection
# JSON body injection: test if __proto__ is accepted
## Phase 3: Prototype Pollution → RCE Chain
# OOB confirmation: check Interactsh for callback
## Phase 4: Express Trust Proxy Abuse
## Phase 5: Template Engine SSTI (EJS / Pug / Handlebars)
# EJS SSTI: if user input reaches EJS template context
# Handlebars: prototype pollution via template
## Phase 6: child_process Command Injection
## Phase 7: /proc/self/environ Exfil
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.