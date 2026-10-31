**Pattern 1; Omit the CAPTCHA field (most common, most automatable):**
**Pattern 2; Empty or null CAPTCHA value:**
**Pattern 3; Replay a previously solved CAPTCHA token:**
**Pattern 4; Test without CAPTCHA on similar endpoints:**
**Pattern 5; Rate/throughput-gated "prove you're automated" challenges:**
Some apps define CAPTCHA "bypass" as simply exceeding the rate a human could plausibly sustain -
e.g. "N submissions within T seconds"; checked by a middleware that counts REQUESTS REACHING the
**Timing note (sliding-window counters):** don't solve this one request at a time; a sequential
- **`hunt-brute-force`**; CAPTCHA is often the only rate-limit gate; bypass unlocks brute force
- **`hunt-forgot-password`**; reset endpoints sometimes protected by CAPTCHA only
- **`hunt-race-condition`**; race the CAPTCHA validation window (submit before the token is revoked)
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.