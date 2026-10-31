leak into that client-visible surface; hardcoded in JavaScript, tucked into HTML
files that were never meant to be public; anyone can read them with `curl` and a
visitor; it does not exploit, brute-force, or mutate anything.
Also scan any sourcemaps (`*.js.map`); they can rebuild original source with
key the same way; report the pattern.
- ✅ Flag secrets for **rotation**; served/committed secrets are already burned.
- ❌ Don't treat "it's a test/demo key" as safe; report the shipping pattern.
  auth logic, IDOR, or server-side flaws; pair with `@web-security-testing`.
  the server already serves publicly; no exploitation, brute force, or mutation.
- Do not exfiltrate data or reuse discovered credentials; locating an exposure is
  **Solution:** Report the pattern; the same code path would ship a live key.
- `@web-security-testing`; broader OWASP Top 10 workflow; use for injection,
- `@dependency-management-deps-audit`; pairs well for supply-chain/component risk