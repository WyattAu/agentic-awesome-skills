# Shopify Review Triage: public low-star reviews to a P0–P3 brief
It is built for independent Shopify app teams and the agencies that run their support; the
  prioritized, or clustered; even when they never say "triage", "severity", or "P0".
- Do **not** use it to gather reviews, contact reviewers, or publish replies; see the hard
   URL that was not supplied. A row with no link gets `source: not captured`; never a guessed one.
   labeled *first pass; not human-checked*. Only a person who read the review and checked it
7. **Draft only; never contact anyone.** Do not send email, post a developer reply, open a
of placing it in a priority bucket or in competitor watch; a guessed owner is exactly the kind
The shorter form used by the free worksheet is also fine; treat field 1 as the rating when it
### Step 2: First pass: apply the rubric
Five buckets. Each row gets exactly **one primary** bucket; the first dimension below, in this
**Suggested action.** No keyword matched. Read the full review yourself and file it manually; the heuristic makes no guess here.
provisional placement in the queue, not as a severity judgment; nothing has been judged yet.
   positioning, or copy input; never your P0.
### Step 3: Human pass: verify before you promote anything
- record the outcome as *reproduced*, *not reproduced*, or *attempted; notes attached*.
*first pass; not human-checked*, including in the summary line. An unverified P0 is a candidate,
# Low-star review brief: {portfolio or team name}: week of {YYYY-MM-DD}
Covers only the rows supplied; no claim of exhaustive coverage.
## P0: Incident risk
- **{App}; {signal in a few words}** ({rating}★, {review date}, source: {public reviews URL or not captured})
  - Status: first pass; not human-checked / human-checked
  - Reproduced: {yes / no / attempted; notes}
  - Next action: {action}; owner {name}, due {date}
## P1: Repeated friction
- **{App}; {theme}** ({rating}★, {date}, source: {public reviews URL or not captured}; also seen: {where})
  - Status: first pass; not human-checked / human-checked
  - Next action: {UX or docs change}; owner {name}, due {date}
## P2: Pricing confusion
- **{App}; {signal}** ({rating}★, {date}, source: {public reviews URL or not captured})
  - Status: first pass; not human-checked / human-checked
  - Next action: {copy or prompt change}; owner {name}, due {date}
## P3: Feature requests
- **{App}; {request}** ({rating}★, {date}, source: {public reviews URL or not captured}); {log it / already exists → reply with where to find it}
- **{App}** ({rating}★, {date}, source: {public reviews URL or not captured}); {no keyword matched; what a human should look for}{, or: ownership: not supplied; app name on neither list}
- **{Competitor}; {signal}**: {what it implies for our roadmap, copy, or positioning}
2 repeated friction, 1 pricing confusion, 1 feature request, 1 needs human read; first pass,
- [ ] Every unverified item says *first pass; not human-checked*; nothing claims a human check
- [ ] Nothing was sent, posted, or published; the brief is a draft for the team.
### Example 1: Worked example: eight rows in, first pass out
**Explanation:** Rows 4 and 5 both matched `confusing`, so they are flagged as a repeated theme -
## P0: Incident risk
- **Example Popup App; editor reported blank, popup reported not loading** (1★, 2026-07-28, [source](https://apps.shopify.com/example-popup-app/reviews?ratings%5B%5D=1))
  - Status: first pass; not human-checked
  - Next action: attempt reproduction on a development store today; owner {name}, due {date}
a report ("the reviewer reports"), the status stays *first pass; not human-checked* until a
filter kept; the App Store has no per-review permalink.
- ✅ **Do:** label every unverified item *first pass; not human-checked*, including in the summary.
- ❌ **Don't:** send, post, or publish anything; including a developer reply to a reviewer.
- **Problem:** `checkout` is the noisiest keyword in the set; it fires on "we love the checkout
- **Problem:** `missing` and `error` cross buckets; "missing a dark mode" is P3, "settings page
- `@customer-research`; when the goal is broader voice-of-customer synthesis rather than
- `@shopify-apps`; when the next step is actually building or fixing the Shopify app behavior a
- `@before-you-build`; when a P3 feature request needs product-risk review before it becomes