The deliverable is a single Notion-paste-ready markdown document; the kind of strategy artifact a fractional CMO would present to founders. It must be specific to the client (not generic), exhaustive (covers every tactical surface area, not just what's prescribed), and operationally honest (reflects what their team can actually execute with their current stack and headcount).
**Do not use** when the user wants a tactical execution document for a single channel (use the channel-specific skill instead; `email-sequence`, `paid-ads`, `seo-audit`, `onboarding`, etc.), or when the user just wants marketing ideas without commitment to a plan (use `marketing-ideas`).
On invocation, the skill reads `~/marketing-plans/{client-slug}/progress.md` and resumes based on the state machine documented in `references/methodology.md` Step 1.1.2 (fresh → INIT → REVIEW → FINALIZE → finalized). Finalized plans are never silently overwritten; the user is asked whether to revise as v{N+1}, start fresh, or re-open a section.
### Phase 1: INIT (research + intake)
Use the embedded 17-section current-state rubric (`references/current-state-rubric.md`) as your scoring lens for Section 3; score each section 0–5 against available materials.
### Phase 2: REVIEW (walk through each of 13 sections interactively)
Save each confirmed section to the progress file as you go. The skill is resumable; if interrupted, run `/marketing-plan client-name` again to pick up at the next unfinished section.
### Phase 3: FINALIZE (compile + verify + publish)
1. **Executive summary**; 3 big bets, 90-day priorities, 12-month outcome. Written so it can be lifted into an investor or board update.
2. **Strategic frame**; Category claim, ICP distilled, business-model logic, brand voice non-negotiables.
3. **Current state**; Team, budget, what's done, what's in-flight, what's stuck. Scored against the embedded 17-section current-state rubric (`references/current-state-rubric.md`).
4. **Acquisition**; How strangers become aware. Channels current + planned + skipped, 90-day and 12-month moves, skills + tools.
5. **Activation**; How a new user has an experience that converts. Onboarding, first session, App Store / signup, paywall, lifecycle setup.
6. **Retention**; How a converted user stays and deepens. Lifecycle flows, churn prevention, win-back, support-as-marketing.
7. **Referral**; How retained users bring more users. Ambassador / affiliate / Guides / WOM mechanics.
8. **Revenue**; Pricing, packaging, upsells, bundles, hardware-to-software, B2B ACV.
9. **90-day roadmap**; Weeks 1–2 (Unblock), 3–4 (Foundation), 5–8 (Velocity), 9–12 (Compound). AARRR-tagged, owner-assigned.
10. **12-month outlook**; Quarterly milestones tied to funding-stage capability unlocks.
11. **Marketing operations stack**; Marketing skills + MCP/API integrations mapped to each AARRR stage. Capability unlocks by funding stage.
12. **Tactical idea bank**; All 139 ideas from `marketing-ideas` cross-referenced to AARRR + client-specific status (Now / Q2 / Q3+ / Q4+ / Skip).
13. **Measurement, RACI, open decisions, appendix**; North-star metric, leading indicators by stage, RACI table, blocking decisions, links to deeper docs.
Brand and content are **cross-cutting**, not their own AARRR stage; they serve every stage.
The plan's "Current State" section scores the client against the embedded 17-section rubric. Full rubric in `references/current-state-rubric.md`; it's the source of truth, not a derivative of any external skill.
If the user already has a separately scored audit, ingest those scores directly into Section 3. Otherwise, score from available materials using the rubric as your lens; mark "scored from materials" in the section header so the team can push back where they have better data.
## Cross-references: skills this plan integrates with
1. **`marketing-ideas`**; 139 marketing ideas (not guarantees of effectiveness). Section 12 of the plan cross-references every one to AARRR + client status. Detail in `references/idea-cross-reference.md`.
2. **`product-marketing`**; Sets up the foundational `.agents/product-marketing.md` context file (positioning, ICP, voice). Read it if available and relevant; Section 2 (Strategic frame) builds on it.
3. **AARRR-stage-specific skills**; `onboarding`, `signup-flow-cro`, `email-sequence`, `referral-program`, `pricing`, etc. The "Marketing operations stack" (Section 11) maps these to AARRR stages.
This is the differentiator of an fCMO-style plan vs. a generic marketing plan. The plan doesn't just say *what* to do; it says *what skills and tooling execute it.*
- **Pre-seed / bootstrapped**; $0–$2K/mo total marketing spend; organic only
- **Seed close**; $5–$15K/mo paid test budget; first marketing hire
- **Seed deployment**; $20–$50K/mo paid; second marketing hire
- **Series A**; $50–$150K/mo paid; performance + content + designer; international consideration
- **Series B+**; $150K+/mo paid; brand campaigns; PR firm; full-stack marketing org
1. **Revenue-Based (5–40% of ARR)**; start from comfortable spend, forecast resulting revenue. Best when historical CAC data exists.
2. **Goal-Based**; reconcile target ending ARR with retained opening ARR, expansion and the survival of new acquisition cohorts; use the explicit definitions/formula in `references/budget-planning.md`. Best for fundraising or when the goal is fixed.
Optionally allocate a **10–20% experimental reserve within the approved total**, with tradeoffs shown; CAC is the main dependency, and the experimental layer is what funds the next-channel investment before the current one plateaus.
## Growth patterns: the real shape of SaaS growth
- **Phase identification**; $0–10K ARR (grueling), $10K–100K (treacherous middle), $100K–1M (acceleration). Section 3 names the current phase; Section 10 sequences the next.
- **Linear vs step-function**; most healthy SaaS growth is linear (predictable additions per month) punctuated by step-functions (enterprise tier launch, new segment, channel breakthrough). The plan should describe both honestly; not promise exponential.
- **S-curve layering**; Channel × Product × Market. Start the next S-curve while the current one is still growing. Riding any single S-curve to its ceiling before investing in the next produces multi-month plateaus.
Strategy lives in-house. Execution can; and often should; be outsourced. Full framework in `references/team-and-agency-model.md`. Three implications for every plan:
1. **Choose the first hire against the demonstrated capability gap.** Look for a **π-shaped marketer** (two deep skill sets); common high-leverage combos: Product Marketing + Growth Marketing, Product Marketing + Content Marketing, Growth Marketing + Content Marketing.
1. **Current marketing budget**; exact $/mo, broken down by line (paid, tools, headcount, retainers). Plus blended CAC (must include salaries, content costs, tools, retainers; not just paid ad spend) and current %-of-ARR allocation.
2. **Unit economics**; ARPC, annual retention rate, LTV. These feed the budget math in Section 8 and Section 10.
3. **Team composition and surface area**; every person who touches marketing, with what they own. Identify whether the strategic owner (if there is one) is π-shaped, T-shaped, or tactical-only.
4. **What the client is currently doing**; by channel, with status (working / not / TBD).
5. **What they've already done that should be acknowledged**; past launches, PR moments, content, partnerships. Don't write a plan that ignores work they're proud of.
6. **Phase of SaaS growth**; $0–10K ARR / $10K–100K / $100K–1M / $1M+. Each phase has its own binding constraint.
7. **Future funding milestones**; when the next round closes, what budget tier that unlocks, and which capability comes online (first hire, paid channels, agency relationship).
8. **The marketing skills mapped to specific moves**; every move in the AARRR sections names the skill that executes it.
9. **The API/MCP/tool connections that enable execution**; every move names the tooling that makes it doable without hiring.
If you can't confirm any of these in INIT, list them in Section 13's "Open decisions"; never gloss over them. **CAC unknown may be a high-impact open decision**; every revenue projection depends on it.
- **B2B SaaS**; Acquisition leans on SEO + content + outbound + LinkedIn. Activation = signup + product trial. Retention = product engagement + CSM motion. Referral = customer advocacy. Revenue = expansion / NRR.
- **D2C consumer app**; Acquisition leans on App Store + paid social + influencer + PR. Activation = onboarding + first session + paywall. Retention = lifecycle email + push. Referral = sharing mechanics. Revenue = subscription + upsell.
- **Hardware-led**; Acquisition leans on PR + retail + Amazon + Shopify SEO. Activation = unboxing + setup + first use. Retention = software companion + community. Referral = gifting + reviews. Revenue = blended LTV hardware + accessories + subscription.
- **Marketplace**; Activation has two sides (supply + demand). Retention is repeat transaction frequency. Revenue is take-rate × GMV.
- **Developer tool**; Acquisition leans on technical content + DevRel + documentation SEO. Activation = first build / first integration. Retention = depth of integration. Referral = team adoption.
- The exec summary can stand alone; could be lifted into an investor update
- Ignoring brand voice; every plan section must respect the client's voice rules
    ├── progress.md        # State machine; phase, current_section, approved artifacts, plan_version
- **`product-marketing`**; Optional if installed. Captures positioning, ICP, voice in `.agents/product-marketing.md` so every section of the plan references the same foundation.
- **`marketing-ideas`**; Source of the 139 tactics in Section 12.
- **`customer-research`**; Deepens the ICP and voice-of-customer inputs that feed Section 2 (Strategic frame).
- **`onboarding`**; Deep work on Section 5 (Activation).
- **`email-sequence`**; Deep work on Section 6 (Retention) + onboarding emails in Section 5.
- **`referral-program`**; Deep work on Section 7 (Referral).
- **`pricing`**; Deep work on Section 8 (Revenue).
- **`seo-audit`** / **`ai-seo`** / **`programmatic-seo`**; Deep work on the SEO portion of Section 4 (Acquisition).
- **`paid-ads`** / **`ad-creative`**; Deep work on the paid portion of Section 4 once budget unlocks.
- **`launch-strategy`**; Deep work on launch moments inside Section 4 / Section 9.
1. **Funding state**; What round are you in? How much raised so far? Burn? Runway? Upcoming rounds and timing?
2. **Team**; Who are all the people who touch marketing? What does each own? Where are the gaps?
3. **Budget**; What's the current monthly marketing spend, broken down by paid acquisition, tools, retainers, headcount? What budget unlocks when the next round closes?
4. **Current channels**; What's working today? What's not? What have you not tried yet?
5. **Already done**; What past campaigns / launches / content / PR moments should this plan acknowledge?
6. **In-flight**; What's drafted but not shipped? What's blocking each item?
7. **Tooling stack**; What's wired? Customer.io / Mailchimp / Resend? Shopify / Stripe / App Store Connect? GA4 / Mixpanel / Amplitude? GitHub / Notion / Figma?
8. **Beta or GA?**; If product is in beta, what's the GA timeline? Throttling? What gates exist?
9. **The most important thing to fix this quarter**; founder's read.
10. **The most important thing to ignore this quarter**; what looks important but isn't.
That said: don't pad. Every section should be **dense, not bloated**. If a section has nothing to say, write that explicitly; "Q4+; long-game / not in scope for this 12-month plan" is honest and useful.