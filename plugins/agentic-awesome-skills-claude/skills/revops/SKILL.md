1. **GTM motion**; Product-led (PLG), sales-led, or hybrid?
2. **ACV range**; What's the average contract value?
3. **Sales cycle length**; Days from first touch to closed-won?
4. **Current stack**; CRM, marketing automation, scheduling, enrichment tools?
5. **Current state**; How are leads managed today? What's working and what's not?
6. **Goals**; Increase conversion? Reduce speed-to-lead? Fix handoff leaks? Build from scratch?
Work with whatever the user gives you. If they have a clear problem area, start there. Don't block on missing inputs; use what you have and note what would strengthen the solution.
Every handoff between teams is a potential leak. Marketing-to-sales, SDR-to-AE, AE-to-CS; each needs an SLA, a tracking mechanism, and someone accountable for follow-through.
- **Fit score**; Does this person match your ICP? (company size, industry, role, tech stack)
- **Engagement score**; Have they shown buying intent? (pricing page, demo request, multiple visits)
**Explicit scoring (fit)**; Who they are:
**Implicit scoring (engagement)**; What they do:
**Negative scoring**; Disqualifying signals:
5. Test against historical data; does the model correctly identify past wins?
- Include a **fallback owner**; unassigned leads go cold fast and waste pipeline
- **Required fields per stage**; Don't let reps advance a deal without filling in required data
- **Stale deal alerts**; Flag deals that sit in a stage beyond the average time (e.g., 2x average days)
- **Stage skip detection**; Alert when deals jump stages (Qualified → Proposal skipping Discovery)
- **Close date discipline**; Push dates must include a reason; no silent pushes
- **Lifecycle stage updates**; Auto-advance stages when criteria are met
- **Task creation on handoff**; Create follow-up task when MQL assigned to rep
- **SLA alerts**; Notify manager if rep misses response time SLA
- **Deal stage triggers**; Auto-send proposals, update forecasts, notify CS on close
- **MQL alert**; Instant notification to assigned rep with lead context
- **Meeting booked**; Notify AE when prospect books via scheduling tool
- **Lead activity digest**; Daily summary of high-intent actions by active leads
- **Re-engagement trigger**; Alert sales when a dormant lead returns to site
- **Round-robin scheduling**; Distribute meetings evenly across team
- **Routing by criteria**; Send enterprise leads to senior AEs, SMB to junior reps
- **Pre-meeting enrichment**; Auto-populate CRM record before the call
- **No-show workflows**; Auto-follow-up if prospect misses meeting
Document every exception. Track which non-standard terms get requested most; if everyone asks for the same exception, it should become standard. Review quarterly.
- **Matching rules**; Email domain + company name + phone as primary match keys
- **Merge priority**; CRM record wins over marketing automation; most recent activity wins for fields
- **Scheduled dedup**; Run weekly automated dedup with manual review for edge cases
- Use progressive profiling; don't require everything upfront
1. **Marketing view**; Lead volume, MQL rate, source attribution, cost per MQL
2. **Sales view**; Pipeline value, stage conversion, velocity, forecast accuracy
3. **Executive view**; CAC, LTV:CAC, revenue vs. target, pipeline coverage
1. **Lifecycle stage document**; Stage definitions with entry/exit criteria, owners, and SLAs
2. **Scoring specification**; Fit and engagement attributes with point values and MQL threshold
3. **Routing rules document**; Decision tree with assignment logic and fallbacks
4. **Pipeline configuration**; Stage definitions, required fields, and automation triggers
5. **Metrics dashboard spec**; Key metrics, data sources, and target benchmarks