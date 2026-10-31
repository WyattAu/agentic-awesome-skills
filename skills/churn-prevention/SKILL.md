1. **Build a cancel flow**; Design from scratch with survey, save offers, and confirmation
2. **Optimize an existing flow**; Analyze cancel data and improve save rates
3. **Set up dunning**; Failed payment recovery with retries and email sequences
| Business closed | Skip offer (respect the situation) |; |
| Approaching plan limit | Upgrade nudge (not a wall; paywall-upgrade-cro handles this) |
| Hard decline (permanent) | Card stolen, account closed | Don't retry; ask for new card |
| 2 | Day 3 | Helpful reminder | "Quick reminder; update your payment to keep access." |
- **Acquisition channel**; Which channels bring stickier customers?
- **Plan type**; Which plans churn most?
- **Tenure**; When do most cancellations happen? (30, 60, 90 days?)
- **Cancel reason**; Which reasons are growing?
- **Save offer type**; Which offers work best for which segments?
**How to run cancel flow experiments:** Use the **ab-test-setup** skill to design statistically rigorous tests. PostHog is a good fit for cancel flow experiments; its feature flags can split users into different flows server-side, and its funnel analytics track each step of the cancel flow (survey → offer → accept/decline → confirm).
- **Unmeasured retention offers**; Direct cancellation is a valid design. Test optional offers against later paid retention, margin, complaints and time needed to cancel.
- **Making cancellation hard to find**; Hidden cancel buttons breed resentment and bad reviews. Check applicable current consumer and auto-renewal requirements; the FTC's 2024 amended rule was vacated in July 2025, as documented in its March 2026 notice
- **Same offer for every reason**; A blanket discount doesn't address "missing feature" or "not using it"
- **Discounts too deep**; 50%+ discounts train customers to cancel-and-return for deals
- **Ignoring involuntary churn**; Measure its share and address recoverable failures
- **No dunning emails**; Letting payment failures silently cancel accounts
- **Guilt-trip copy**; "Are you sure you want to abandon us?" damages brand trust
- **Not tracking save offer LTV**; A "saved" customer who churns 30 days later wasn't really saved
- **Pausing too long**; Choose pause limits from your own customer needs and return data.
- **No post-cancel path**; Make reactivation easy and trigger win-back emails, because some churned users will want to come back