Cron is deceptively error-prone. The failure mode is **silent**; a syntactically
**OR** every Monday"; ~6 fires/month instead of ~2.
- Use when a user writes, edits, reviews, or deploys a cron expression; in a
schedule behaves as expected. Estimate annual fire count; a schedule that fires
### 1. Impossible dates: the "never fires" bug
### 2. OR-semantics: the "fires too often" bug
### 3. Midnight spike: the "everything at once" bug
backups, log rotations, cert renewals, report generation; all fire simultaneously.
### 4. Uneven steps: the "drift" bug
0, then resets at 60." So: 0, 7, 14, 21, 28, 35, 42, 49, 56; then 0 again
### 5. Leap-year February 29: the "annual surprise"
Fires only on leap years; February 29, 2024 / 2028 / 2032… If someone writes this
// Programmatic; Node.js, zero dependencies
- ❌ Don't trust `crontab -l` validation; it lists the installed crontab; it is not a syntax or semantic validator.
- **Problem:** "Intervals are uneven; sometimes 7 min, sometimes 4."
- `docker-expert`; when the cron job runs inside a container and the issue is the
- `kubernetes-deployment`; when validating a `CronJob` manifest's `spec.schedule`
writes, network calls, or mutations; it only parses and computes. It accepts data only, but inputs/counts must be bounded and execution should use reviewed local files. Parsing cannot guarantee a job will execute.