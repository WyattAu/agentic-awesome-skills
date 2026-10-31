**Start with username enumeration; it's the fastest win and gates the rest.**
**Pattern 1; Username enumeration (response difference for valid vs invalid email):**
1. POST to the forgot-password endpoint with a clearly invalid email (e.g. `nonexistent@fakedomain12345.com`); record the response body, status code, and length
**Pattern 2; Reset token exposed in the API response:**
**Pattern 3; Reset token replay (reuse after use):**
**Pattern 4; No rate limit on reset requests:**
- `base64(email + timestamp)`; decodable
- 4-6 digit numeric code; 10,000 to 1,000,000 possibilities before considering expiry and server-side attempt limits
- Sequential `token=1234`, `token=1235`; trivially enumerable
- **`hunt-ato`**; owns the account-takeover CHAIN (password-reset is its path #1). This skill finds/proves the recovery-flow primitive; hand off to hunt-ato to assemble the full takeover.
- **`hunt-cache-poison`**; host-header injection during reset email generation (different vulnerability, same flow)
- **`hunt-brute-force`**; rate-limit testing pattern applies to the reset endpoint too
- **`hunt-auth-bypass`**; if the reset flow can be skipped entirely (go to `/reset-password?token=` with empty/null token)
- **`hunt-mfa-bypass`**; if MFA is required after reset, test the bypass there
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.