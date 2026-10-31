# Verification: Phase 2H: cache poisoning + HTTP smuggling
> Two-server lab (Nginx 1.27 proxy + gunicorn/Flask origin) via Docker. Cache poisoning succeeded; HTTP smuggling **didn't**; and the failure mode is exactly the kind of honest negative that updates the skill content.
## Test 19: cache poisoning via X-Forwarded-Host (`hunt-cache-poison`)
**Skill that auto-triggers:** `hunt-cache-poison`; description includes "unkeyed inputs (X-Forwarded-Host, X-Original-URL, custom headers)".
  <h1>Welcome; assets load from attacker.evil</h1>
The origin reflected `attacker.evil` into the response. Nginx cached this response keyed by `$scheme$request_method$host$request_uri` only; NOT by `X-Forwarded-Host`.
  <h1>Welcome; assets load from attacker.evil</h1>
**`X-Cache-Status: HIT`**; victim is served the poisoned response from the cache. The CSS / JS URLs now point at `https://attacker.evil/`. Victim's browser will fetch attacker-controlled assets.
**PASS; live cache poisoning + XSS chain.** Exact technique from `hunt-cache-poison`. `triage-validation` 7-Question Gate passes all 7; Critical.
## Test 20: HTTP smuggling on Nginx 1.27 (`hunt-http-smuggling`)
> "Nginx in front of gunicorn; testing for CL.TE / TE.CL smuggling."
**Skill that auto-triggers:** `hunt-http-smuggling`; description includes "CL.TE, TE.CL, H2.CL, H2.TE", "obfuscation variants (space before colon, trailing space, lowercase, mixed-case)".
The tab-separator variant returned `501 Not Implemented`; Nginx parses the encoding name `\tchunked` and rejects it as unknown rather than treating it as the same encoding as `chunked`. Either way, smuggling fails.
## Summary: Phase 2H
| 19 | Cache poisoning via X-Forwarded-Host | `hunt-cache-poison` | PASS; victim served attacker's CSS/JS URLs from cache |
| 20 | HTTP smuggling on Nginx 1.27 | `hunt-http-smuggling` | **Negative; Nginx hardened**. Gap closed with target-suitability matrix. |
Containers ephemeral; no persistence needed.