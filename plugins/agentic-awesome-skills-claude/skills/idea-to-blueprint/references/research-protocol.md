Run the tracks in order. C before D is not cosmetic: the shape of the system constrains which technologies can serve it, and choosing the technology first means the architecture gets reverse-engineered to fit a decision that was never tested against the product's requirements. D before E for the same reason; the quality tooling that exists depends on the language you picked.
- Use the current year in queries about "latest", "current", "best"; stale year terms return stale pages.
| `[VERIFIED; S-07, 2026-09-05]` | You read a primary or strong secondary source that states it | "Framework X current stable is 15.2 [VERIFIED; S-07, 2026-09-05]" |
| `[ASSUMED; reason]` | Your choice where nothing was stated or found; reason in one clause | "Single currency (Toman) [ASSUMED; all benchmarked local competitors price in Toman]" |
| `[VERIFY-AT-BUILD]` | True now but volatile (service availability, sanctions, prices); the building session must re-check | "SMS provider Z reachable from EU hosting [VERIFIED; S-12, 2026-09-05] [VERIFY-AT-BUILD]" |
3. Reputable independent reports and well-known technical publications; corroborate with a tier-1 source when the claim drives a decision.
4. Reviews, forums, app-store comments; only for *user pain and expectations*, paraphrased, never quoted, never as fact about features.
5. Blogs and listicles; leads only; never cite as the sole support for a decision.
## 4. Track A: Understand the idea
## 5. Track B: Benchmark
5. **Gap and positioning**: one paragraph; the unmet need or under-served persona, and the positioning statement: "For <persona> who <need>, <product> is a <category> that <benefit>, unlike <alternative> which <limitation>."
## 6. Track C: Architecture drivers and shape
2. **Shape before technology.** Score 2–3 candidate architecture styles against those drivers. Record the scoring table; it becomes a DR. The default for this skill's output is a modular monolith with enforced boundaries; but "default" means it must still be argued against the drivers and given explicit exit triggers, not asserted.
4. **Verify fitness-function tooling** for the candidate languages; a maintained architecture-testing library, or the lint mechanism that stands in for one. This determines whether boundaries can be enforced or only described, which is a real input to the style decision.
5. **Category-specific architectural requirements**; payment callback and reconciliation models, webhook vs polling for messaging platforms, audit-trail requirements, anything the domain forces on the shape.
## 7. Track D: Stack for agentic engineering
The stack is chosen for one reader: a coding agent working alone in a fresh session, who must produce correct code with no one watching. That changes the usual criteria. Score candidates against the weighted table below **and** against the architecture drivers from Track C; a stack that wins generically but cannot meet a rank-1 driver loses.
2. For every component of every candidate, verify **this session**: current stable version and release date, LTS/maintenance status, not archived/deprecated, license, RTL/i18n support if needed, and the exact install/init command from official docs. Tag each `[VERIFIED; S-nn, date]`.
3. Score the matrix, pick, and write the **decision record**: chosen stack with pinned versions; the runner-up and why it lost; explicitly **forbidden choices** with reason (e.g., "no ORM X; archived [VERIFIED]", "no service Y; requires foreign card / blocked for Iranian IPs [VERIFIED] [VERIFY-AT-BUILD]").
### Heuristics (not rules: verify every time)
## 8. Track E: Quality bar and tooling
1. **The gate components**; formatter, linter and the community-standard strict rule set, typechecker and its strict flags, test runner, coverage tool, mutation-testing tool (does a maintained one exist for this language, or not?), SAST/dependency audit, secret scanning, duplication detector, accessibility checker, architecture-rule library. Get the exact commands from official docs, not from memory.
3. **CI reality on the chosen host**; runners, cache, secret storage, required-check/branch-protection capability, and what the plan the user can actually buy includes.
4. **The security baseline for the category**; the current OWASP Top 10 (or API/mobile equivalent), plus anything the domain adds (payments, health, minors, personal data).
5. **Current delivery-metric definitions** if the blueprint will name any; the present DORA formulation and its performance bands. The framework has changed more than once; do not quote a version from memory.
## 9. Track F: Integrations and locale realities
- Sandbox/test mode availability; the acceptance criteria will require testable integrations.
- Webhook/callback model, idempotency guarantees, rate limits, error codes; these become edge cases and AC.
## 10. Track G: UX benchmarks
- Foreign services commonly blocked for Iranian IPs or requiring foreign cards (fonts CDNs, analytics, maps, package registries mirrors, container registries, error tracking, email delivery); and the domestic or self-hosted alternative for each one you need.
- Time zone (Asia/Tehran) and current DST policy; official holiday calendar source; work week (Saturday–Wednesday full, Thursday half or off in many sectors; verify for the segment); Jalali calendar library options in the chosen stack (maintained, correct leap-year handling).