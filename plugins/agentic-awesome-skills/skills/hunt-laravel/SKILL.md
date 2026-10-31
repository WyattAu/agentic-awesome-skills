# HUNT-LARAVEL: Laravel Specific Vulnerabilities
- **Ignition RCE (CVE-2021-3129)**; `APP_DEBUG=true` + Laravel < 8.4.2 → `/_ignition/execute-solution` RCE without auth
- **Telescope dashboard**; `/telescope` exposes full request/response logs, DB queries, Redis commands, scheduled jobs, environment variables
- **Horizon dashboard**; `/horizon` exposes queue job details, failed jobs with full payloads (may contain API keys, PII)
- **Signed URL manipulation**; if `URL::signedRoute` validates wrong params → bypass signed URL → unauthorized actions
- **.env exposure**; `APP_KEY` leaked → decrypt all encrypted cookies → forge session → ATO
## Phase 1: Fingerprint Laravel
## Phase 2: Debug Mode & Ignition RCE (CVE-2021-3129)
# Step 3: CVE-2021-3129: Laravel < 8.4.2 RCE via log file manipulation
# Manual test: send solution request
## Phase 3: Laravel Telescope & Horizon
# Telescope: request/response logs, DB queries, jobs, cache, events
# Horizon: queue worker dashboard
## Phase 4: .env File & APP_KEY Exposure
## Phase 4b: Environment override via query args (CVE-2024-52301)
## Phase 5: Signed URL Manipulation
# Test: modify a non-signature parameter: should fail validation
## Phase 6: Mass Assignment via Eloquent
# Laravel Eloquent ORM: if model uses $guarded=[] or $fillable=[] improperly
## Phase 7: Laravel Cookie Deserialization
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.