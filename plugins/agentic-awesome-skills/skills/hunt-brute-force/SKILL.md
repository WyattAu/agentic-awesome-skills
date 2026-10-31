# HUNT-BRUTE-FORCE: Rate Limiting / Brute Force / Enumeration
> curated set of named HackerOne reports. `report_count` is intentionally `0`; do
- **OTP / 2FA brute → MFA bypass → ATO**; no effective rate limit on `/verify-otp`, full 000000–999999 keyspace reachable
- **Password-reset token brute**; short/predictable/non-expiring tokens + no rate limit → ATO (the Instagram 2019 case combined a 6-digit reset code, no rate limit per request-source, and IP rotation to make 10^6 tractable)
- **Username/email enumeration → targeted credential stuffing**; valid/invalid distinguishable by response string, status code, or timing, use only controlled synthetic identities; never acquire/spray breach corpora
- **Coupon / gift-card / referral code brute**; no rate limit on code validation → financial impact
- **ReDoS**; attacker-controlled input hits a catastrophic-backtracking regex → CPU exhaustion → DoS
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.