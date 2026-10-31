# Methodology: How a Marketing Plan Gets Made
## Phase 1: INIT (research + intake)
### Step 1.1: Set up the plan folder
├── progress.md        # State machine; see Step 1.1.1 for schema
### Step 1.1.1: `progress.md` state schema
# {Client}: Marketing Plan Progress
### Step 1.1.2: Resumption decision tree
### Step 1.2: Read existing materials
### Step 1.3: Pull scoped read-only data when authorized
### Step 1.4: Conduct structured intake
#### Intake 1: Client overview
#### Intake 2: ICP
#### Intake 3: Funnel state today
- What's the funnel *shape*; is it bottle-necked at top, middle, or bottom?
#### Intake 4: Funding state
#### Intake 5: Team
#### Intake 6: Budget
- Blended CAC if known (including salaries, content costs, tools, retainers; not just paid ad spend). If unknown, flag as the top Section 13 open decision; every revenue projection depends on it.
#### Intake 7: Channels currently active
- Acquisition: organic SEO, paid search, paid social, content, social, partnerships, events, PR, ambassadors, etc.; for each, status (live / paused / never tried)
#### Intake 8: Already done
#### Intake 9: In-flight and stuck
#### Intake 10: Strategic posture
### Step 1.5: Score current state against the rubric
- **From rich materials.** When the team has shared decks, prior content audits, an existing brand voice doc, recent positioning work, or a kickoff call transcript; score from those. Mark "scored from materials" in the section heading.
### Step 1.6: Write research.md
# {Client}: Marketing Plan Research Record
## Phase 2: REVIEW (section-by-section drafting)
### Step 2.1: Initialize progress.md
### Step 2.2: Walk each section in this order: 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, then 1
1. Present the draft (or key bullets; short sections inline, long sections as bullet outline first)
4. Save the confirmed text to `sections/01.md` ... `sections/13.md` (one file per section, zero-padded for sort order). This is the canonical persisted artifact; recovery depends on it.
### Step 2.3: Section-specific guidance
**Sections 4–8 (AARRR)** each follow the same internal structure: current state, the plan (numbered moves), 90-day moves, 12-month outlook, skills + tools. Don't skip the skills + tools sub-section; it's what makes the plan operationally honest.
### Step 2.4: Brand voice consistency
## Phase 3: FINALIZE (compile + verify + publish)
### Step 3.1: Compile
### Step 3.2: Verification pass
- **Cross-reference check**; every marketing-ideas number (e.g., "idea #17") matches the actual idea in `./idea-cross-reference.md`. Every related-skill mention either exists in the `marketingskills` repo or is documented as an external dependency (see ops-stack-mapping note on cross-marketplace skills).
- **MCP/API check**; every tool mentioned in Section 11 actually exists in the user's stack (per research.md intake) OR is flagged as "future / not yet wired."
- **Path check**; no machine-specific paths (`/Users/...`, `/home/...`) in the output. Replace with descriptive references.
- **Voice check**; final read against brand voice rules. Flag and fix violations.
- **Open-decisions check**; every "TBD" or unanswered question from intake is listed in Section 13's open decisions, not hidden in the body.
- **Acknowledge check**; every item from "already done" in research.md is acknowledged somewhere in the plan.
### Step 3.3: Print
### Step 3.4: Publish (optional)
### Step 3.5: Mark finalized
Resumption is governed entirely by the decision tree in Step 1.1.2 above; always check state in that order on every invocation.
- **Pretending data exists.** If you can't confirm a number (current MRR, retention rate, etc.), don't guess. Mark it `[TBD; to confirm with team]` in the plan and add to open decisions.