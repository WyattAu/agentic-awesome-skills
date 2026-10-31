**Try workflow bypasses before brute force; they're faster and more likely to succeed.**
**Pattern 1; Skip the MFA step entirely (most automatable):**
**Pattern 2; OTP replay (reuse a consumed code):**
**Pattern 3; Submit obviously wrong OTP, observe response:**
**Pattern 4; Partial / incremental validation (prefix oracle):**
1. Submit a 1–3 digit value (e.g. `otp=1`, then `otp=12`, …); for a POST verify endpoint the code goes in the **request body**, not the URL query string, or the server reads an empty value.
3. Walk the code digit-by-digit: keep the prefix that "responds correct," append 0–9, repeat. This collapses 10^6 brute force to ~10×N guesses (≤60 for a 6-digit code); very feasible in a bounded test.
This is the go-to when there is no leaked code and no skip/replay path. Some apps award success on *any* correct prefix outright (so a single correct first digit can win; sweep `otp=0,1,…,9` before giving up).
**CRITICAL; stay in ONE session:** re-authenticating (POST /…/login again) regenerates the OTP, throwing away your prefix progress. Do the entire sweep against a single established MFA session; never re-login between guesses.
**On full brute force:** brute-forcing all 10^6 codes is infeasible in a bounded test; but the prefix oracle above (Pattern 4) usually makes it unnecessary. Only attempt full brute force with evidence of no rate limit AND a small key space.
> Growing bug class; 7 distinct patterns. Pays High/Critical when it enables ATO without prior session.
# Test with ffuf: all 1M 6-digit codes
# -t 5 (slow down): aggressive rates get 429 or ban
        # window before the server marks it used. Two requests are NOT enough -
- **`hunt-ato`**; MFA bypass is a primitive; ATO is the destination. Chain primitive: cookie theft (via XSS or session-fixation) + password oracle (login response timing/length diff reveals valid passwords without lockout) + no MFA step-up on password-change endpoint = persistent ATO without ever facing the OTP challenge → password rotated, attacker locks victim out.
- **`hunt-race-condition`**; Pattern 5 (OTP race) lives in race-condition territory; load both skills together. Chain primitive: same 6-digit OTP submitted via 20 parallel HTTP/2 streams (single-packet Turbo Intruder attack) before the server marks it used → 1 success + 19 "already-used" is expected rejection of repeats, not proof of a race or account takeover.
- **`hunt-auth-bypass`**; MFA-step-skip is auth-flow bypass at the workflow layer. Chain primitive: pre-MFA cookie issued after password step + direct navigation to `/dashboard` skipping `/mfa` route + server only middleware-gates `/mfa` not `/dashboard` = full post-auth access from password-only state → MFA never enforced because the route gate was misplaced.
- **`hunt-misc`**; Recovery-code dump via `/api/me` is a misc-class info disclosure that becomes Critical when chained. Chain primitive: `/api/me` returns full user object including `backup_codes` array (plaintext, never rotated) → attacker with any read-IDOR or XSS exfils backup codes → uses one backup code → MFA satisfied → ATO without OTP knowledge.
- **`security-arsenal`**; Pull the OTP-brute-force payload section (000000-999999 wordlist generator, ffuf rate-limit-evasion patterns with `-t 5 -p 0.5-2`, distributed-IP rotation via proxychains) and the JWT-token-replay table when "MFA satisfied" claim lives in a JWT claim that can be forged.
- **`triage-validation`**; Run the Pre-Severity Gate before claiming Critical on an MFA bypass that only works when the attacker already has the password. Standalone MFA bypass is High; chained-with-password-oracle is Critical; chained-with-cookie-theft-only is Critical. The chain question separates the two.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.