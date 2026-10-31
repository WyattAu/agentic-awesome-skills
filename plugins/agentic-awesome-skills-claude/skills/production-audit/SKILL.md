A skill that runs an external audit on a shipped repo's deployed state; live URL, GitHub signals, secrets exposure, RLS gaps, webhook idempotency, indexes, observability, prompt injection, and ten other failure modes that AI-assisted projects routinely miss.
- During active in-session coding; use `security-review` / OWASP-style for line-level patterns. This skill is for post-merge / pre-ship review.
- For library / scaffold-form repos; the engine handles **app form** best; libraries get a partial-substitute score.
- Inside a private / non-GitHub repo; the audit pulls public GitHub signals, so private repos return a `not_found` error.
If the user pointed at a remote URL instead of `.`, swap `.` for the URL; keep the same `mkdir -p` + version pin + stderr split:
  ↓ [Security] No API rate limiting on /auth; IP cap missing
  ↓ [Infrastructure] webhook handler at api/stripe.ts; signature verified, but no
- Use the exact bullet from `concerns[].bullet`; the audit engine already wrote action-oriented copy.
- Always end with a follow-up question that names a specific concern. Don't ask "what do you want to do?"; ask "fix X first?".
3. Propose a minimal patch; single-file when possible.
    column lacks SELECT grant; silent 42501 on every read
    idempotencyKey; duplicate-charge surface
- ✅ Always cite the exact bullet from `concerns[].bullet`; they're already action-oriented
- ❌ Don't apply fixes without approval; show diff first
- The audit engine is calibrated for **deployed apps** with a live URL. CLI / library / scaffold form gets a partial-substitute score (max ~45/50 on the audit pillar); fair but not flattering.
- Behind a corporate firewall blocking `*.supabase.co`, the API call fails. There is no offline mode; the audit relies on the public engine.
- `@security-review`; In-session line-level security patterns. Run alongside this skill, not in place of.
- `@vibesec`; Editor-buffer security review for vibe-coded projects. Different lens.
- `@owasp-security`; OWASP Top 10 coverage during coding. Companion.
- `@trail-of-bits-skills`; CodeQL / Semgrep static analysis. Different layer.