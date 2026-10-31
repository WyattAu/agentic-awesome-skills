## Firing a race: two primitives (tooling-agnostic)
Winning a race needs requests that arrive in the *same* narrow window; sequential sends may miss a narrow window. Use a single-packet / synchronized-send tool: **Burp Repeater** "Send group in parallel"
- **Identical-copies race**; fire N IDENTICAL copies of one request at once (limit-overrun:
- **Different-requests race (partial construction)**; fire a LIST of DIFFERENT requests in one
  where the object exists in a usable state mid-creation. Example (email-verification bypass -
- **Monetary/credit systems**; double-spending gift cards, coupons, referral bonuses, promotional credits, wallet balances
- **Vote/reputation manipulation**; upvoting the same content multiple times, gaming leaderboards or trending algorithms
- **Account limits bypass**; exceeding free-tier quotas, bypassing "one per user" restrictions on invites, trial activations, or API key generation
- **Privilege escalation**; racing role assignment or permission checks during user creation/upgrade flows
- **Deletion bypass**; reading or exfiltrating data during a narrow window between "marked for deletion" and "actually deleted"
- **Payment flows**; charging a card once but receiving multiple fulfillments
- **Ruby on Rails** without `with_lock` / `lock!`; ActiveRecord doesn't lock by default
- **Node.js** with async/await chains; non-atomic DB reads then writes
- **PHP** without `SELECT ... FOR UPDATE`; common in legacy codebases
- **Microservices**; inter-service calls introduce natural TOCTOU windows
- **Message queues**; idempotency keys often missing
1. **Enumerate one-time or limited-use actions**; Map every endpoint that enforces a "once per user", "limited quantity", or "deduct balance" constraint. These are your primary targets.
2. **Understand the state machine**; For each target action, identify: (a) what state is read, (b) what state is written, (c) what validation sits between read and write. The gap between read and write is your window.
3. **Capture a clean baseline request**; Perform the action once legitimately with Burp Suite intercepting. Confirm you get the expected single-use behavior (e.g., coupon marked used, vote counted once).
4. **Set up parallel request tooling**; Use one of:
5. **Execute the race**; Start with two requests in a controlled lab, increasing only within explicitly approved limits. Key technique: **pre-connect and buffer all requests, release the final byte of all simultaneously** (single-packet attack when HTTP/2 is available).
6. **Analyze responses**; Look for:
7. **Verify the effect**; Check the actual state: Was the credit applied twice? Did the vote count increment multiple times? Is the coupon still marked unused despite two successes?
8. **Determine exploitability window**; Re-run with decreasing parallelism (5 requests, 3 requests, 2 requests) to understand how tight the window is and reliability of exploitation.
9. **Test across account types**; Sometimes the race only works for new accounts, specific subscription tiers, or under specific server load. Test varied conditions.
10. **Document reproducibility**; Record exact timing, number of parallel requests needed, and success rate across 5 independent attempts before reporting.
### Turbo Intruder: Basic Parallel Race
### curl: Parallel Requests (bash)
1. **Check-Then-Act without atomic operations**; Developer reads state (`if voucher.used == false`), then writes state (`voucher.update(used: true)`) in two separate database operations. Any thread can read the same "unused" state before either writes.
2. **Missing database-level locking**; Using ORM methods like `find` or `filter` instead of `SELECT ... FOR UPDATE`. Correct locking requires a transaction, appropriate isolation and coverage of side effects.
3. **Optimistic concurrency without version checking**; Systems increment counters or mark records without checking if the record changed since it was read.
4. **Microservice TOCTOU**; Service A validates eligibility, Service B executes the action. No shared atomic transaction spans both services.
5. **Client-side "protection"**; Developers disable the button in JavaScript after first click, assuming that prevents duplicate submissions. Server-side logic is never hardened.
6. **Counter increments outside transactions**; `votes_count += 1; save()` instead of an atomic SQL `UPDATE SET votes = votes + 1 WHERE id = ?`.
7. **Async background jobs**; Eligibility checked synchronously, fulfillment done asynchronously. A second request passes the check before the first job completes.
8. **Caching without invalidation**; Cached "has user voted?" check returns stale `false` during a cache miss window when the first write hasn't propagated yet.
- Bypass: Rate limits are checked before the action executes. Send requests simultaneously; all pass the rate-limit check before any is counted.
- Bypass: The constraint catches duplicates *after* the race. A valid unique constraint rejects conflicting committed writes; external side effects performed before commit can still need transactional coordination. Look for partial fulfillment; sometimes one succeeds and one errors but both are honored.
- Bypass: Pre-stage all requests with valid tokens. Use single-packet HTTP/2 to release all in one TCP frame; server processes them in the same scheduler slot.
   Can you demonstrate; with screenshots or logs; that the same one-time action succeeded more than once? (e.g., vote count shows +2 from one user, credit balance shows double-credit, coupon shows redeemed twice)
A bug bounty platform's "popular reports" feature allowed upvotes to improve report visibility and researcher reputation scores. By sending ~15 parallel upvote requests for the same report using a single HTTP/2 connection (single-packet attack), a researcher was able to register 10–15 votes from a single account. This allowed artificial inflation of report rankings, manipulation of researcher reputation scores, and distortion of the platform's crowdsourced prioritization system; directly undermining trust in the platform's core feature for triaging vulnerability reports.
### Scenario 2: Major Social Network: Duplicate Promotional Actions
On a major social network (Facebook-scale), promotional or limited-use actions; such as adding a phone number for a one-time security credit, or claiming a one-time bonus; were vulnerable to simultaneous parallel requests. An attacker could race the claim endpoint and receive the promotional benefit multiple times, causing direct financial loss to the platform and allowing fraudulent accumulation of platform currency or benefits at scale. Given the user volume, even a brief window before patching represented significant financial exposure.
### Scenario 3: Cloud Infrastructure Provider: Resource Limit Bypass
A cloud hosting provider enforced limits on the number of resources (e.g., droplets, projects, or API keys) a free-tier user could create. The limit check and resource creation were non-atomic operations. By racing the creation endpoint with 20 simultaneous requests, an attacker bypassed the enforcement logic and created resources far exceeding their tier limit. This translated directly to unauthorized compute consumption, billing fraud, and abuse of infrastructure; impacting both the provider's revenue and system stability for legitimate users.
## Disclosed Report Citations (Backfill +9: 2016-2024)
The following imported report summaries are unverified leads: independently read each linked disclosure before repeating payload, root-cause, severity, patch-version or payout claims. Four cases (#4, #11, #12, plus the bonus reference) use the modern **HTTP/2 single-packet attack** technique (Kettle DEF CON 31, 2023; Flatt Security expansion 2024); the technique that makes most modern race exploits viable today.
4. **GitLab; CVE-2022-4037 email-verification race (Kettle DEF CON 31 case study)** ([NVD](https://nvd.nist.gov/vuln/detail/CVE-2022-4037) · [PortSwigger Research](https://portswigger.net/research/smashing-the-state-machine))
    - Single-packet HTTP/2: **YES**; flagship case study in "Smashing the State Machine"
5. **Worldcoin (Tools for Humanity); World ID action-verification race** ([Medium writeup](https://medium.com/@gonzo-hacks/the-fast-and-the-curious-finding-a-race-condition-in-worldcoin-621c89bfbd61))
    - Year: 2023; **$3,000** (High)
6. **Stripe; Promotion code redeemed past limit** ([H1 #1717650](https://hackerone.com/reports/1717650))
    - Year: 2022; **$250**
7. **Stripe; Fee discounts redeemed many times** ([H1 #1849626](https://hackerone.com/reports/1849626))
    - Year: 2023; **$5,000**, ~$600 platform fee loss per redemption
8. **Reverb.com; Gift card multi-redemption** ([H1 #759247](https://hackerone.com/reports/759247))
    - Year: 2019; **$1,500** (foundational/widely cited)
9. **Cosmos / Starport faucet; Double-mint race** ([H1 #1438052](https://hackerone.com/reports/1438052))
    - Year: 2022; **$5,000** (CVSS 9.3)
10. **InnoGames; Email-activation race → unlimited diamonds** ([H1 #509629](https://hackerone.com/reports/509629))
    - Year: 2019; **$2,000**
11. **RyotaK / Flatt Security; "First Sequence Sync" PIN-bruteforce (10,000-req single-packet expansion)** ([Flatt Security Research](https://flatt.tech/research/posts/beyond-the-limit-expanding-single-packet-race-condition-with-first-sequence-sync/))
    - Single-packet HTTP/2: **YES**; extends Kettle's single-packet from ~30 requests to 10,000 requests in 166 ms by splitting across IP fragments with synchronized TCP first-sequence
    - Year: 2024; **must-reference modern single-packet example**
12. **nopCommerce; CVE-2024-58248 gift-card double-redemption** ([NVD](https://nvd.nist.gov/vuln/detail/CVE-2024-58248))
    - Single-packet HTTP/2: **YES**; single-packet attack reproduces it reliably
- [HTTP/2 Single-Packet Attack; Deep Reference](references/details.md)
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.