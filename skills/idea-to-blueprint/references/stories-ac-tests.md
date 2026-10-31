### E03-S02: <Short imperative title>
**Story**; As **P2 (<persona name>)**, I want <capability>, so that <outcome the persona cares about>.
**Context**; 2–5 sentences: why this exists, what the persona is doing at that moment, what research/benchmark informed it (S-nn), which flow(s) and screen(s) it implements (F-03, SCR-07).
**Scope**; In: … Out (handled in E05-S01 / non-goal): …
**Edge cases**; EC-xxx-nn references plus story-specific ones, each with expected behaviour.
**Tests**; the tests that prove the ACs (see §4).
**Tasks**; ordered implementation tasks E03-S02-T1… (see §4).
**Story DoD**; the checklist that must be true before the story is marked DONE in PROGRESS.md.
Personas are referenced by ID and name; "As a user" is not allowed; if no persona fits, the persona list is incomplete, fix that first. Internal/technical work (migrations, CI) is written as a story for the **builder persona** (define one, e.g., `P0; Builder (the coding agent / maintainer)`), so the format stays uniform and the linter stays happy.
- Is independently testable; one behaviour per criterion; split compound criteria.
Minimum coverage per story (skip a category only when it truly cannot apply, and say why in one word, e.g. "AC-5 permissions: n/a; public screen"):
| Accessibility (UI stories) | Focus order, labels, contrast, touch target size, screen-reader text; as concrete checks |
- Unit: `<module>.<test_name>`; covers AC-2, AC-4 (pure validation and state logic)
- Integration: `<test file>::<test_name>`; covers AC-1, AC-3, AC-8 (through the real DB/test double for the gateway)
- E2E (only if this story is on a critical path listed in section 14): `<spec name>`; covers AC-1, AC-6
- E03-S02-T1 Add `phone` validation to `ENT-User` schema + migration; proves AC-2
- E03-S02-T2 Implement `POST /auth/otp/request` (API-04) with rate limit 5/10min; proves AC-1, AC-3
- E03-S02-T3 Wire SCR-03 form states and copy CP-SCR03-1…6; proves AC-6, AC-7
- E03-S02-T4 Emit `auth.otp.requested` log event without phone number; proves AC-8
- `check` is green on the whole repo; no gate rule weakened, skipped or ignored to get there (if one was, a DR says why).
- Every external symbol used traces to a pinned dependency in the stack table; no invented APIs, no unpinned imports.
## Epic 03: <Title>
**Preconditions for the session**; PROGRESS.md shows E00–E02 DONE; `check` green on the authorized topic branch/worktree; env vars X, Y present; `[VERIFY-AT-BUILD]` items for this epic re-checked: INT-sms availability, …
**Scope**; In: stories below. Out: … (where handled)
### E03-S01: …
### E03-S02: …
**Epic-level tests**; the integration/e2e run that proves the epic as a whole (the flow end to end), plus regression: all previous epics' tests still green.
**Session handoff checklist**; the ≤20-line summary the agent prints before the user terminates (format in session-protocol.md §5).
## 6. Epic 00: Foundation: mandatory contents
2. Formatter, linter (strict), typechecker (strict flags), test runner configured; `check` command that runs them all and fails on any warning class you decide is an error; documented expected runtime. Also in `check` from day one: coverage floor, dependency/vulnerability audit, secret scanning, and schema-drift check; every gate component listed in blueprint section 14.2 that applies.
### E02-S01: Request a one-time code by phone number
**Story**; As **P1 (Restaurant owner, Tehran, mid-range Android, busy)**, I want to enter my mobile number and receive a one-time code, so that I can sign in without remembering a password.
**Context**; Phone-first login is the norm in the benchmarked local products (S-09, S-11) and P1 rarely uses email. Implements F-01 steps 1–3 on SCR-03. SMS delivery through INT-sms (sandbox available [VERIFIED; S-16, 2026-09-05] [VERIFY-AT-BUILD]).
**Scope**; In: number entry, validation, code request, resend cooldown, rate limiting, delivery via provider sandbox in tests. Out: code verification (E02-S02), account creation on first login (E02-S03).
- AC-4 (state edge; rate limit) Given 5 requests for the same number in the last 10 minutes, When a 6th is submitted, Then API-04 returns 429 with `retry_after` seconds, the provider is not called, and SCR-03 shows CP-SCR03-7 including the remaining minutes in Persian digits.
- AC-5 (permissions) n/a; public endpoint; AC-4 is the abuse control.
**Edge cases**; EC-INPUT-03 (invisible characters pasted → stripped before validation), EC-INPUT-07 (mixed Persian/Latin digits → normalised), EC-AUTH-02 (double tap submit → single request, button disabled while pending), EC-NET-04 (offline at submit → CP-SCR03-6 without provider call), EC-MSG-01 (provider accepts but never delivers → covered by resend after 60 s, no more than 3 resends per code), EC-IR-02 (number with operator prefix not in the known list → still accepted; operator list is not a validation rule).
- Unit: `auth/phone.test`; normalisePhone (Persian digits, +98, spaces, ZWNJ, invisible chars), isValidIranMobile; covers AC-2, EC-INPUT-03/07
- Unit: `auth/otp_rate_limit.test`; 5-per-10-min window, retry_after computation; covers AC-4
- Integration: `api/auth_otp_request.test`; success path with provider fake; provider 5xx/timeout; rate limit; idempotent double submit; covers AC-1, AC-3, AC-4, AC-8, EC-AUTH-02
- E2E: `e2e/login.spec` step "request code"; covers AC-1, AC-6 (critical path #1 in section 14)
**Story DoD**; default DoD + manual staging delivery check recorded.