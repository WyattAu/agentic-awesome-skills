Most people describing a project ("vibe coders" included) only give a brief or partial picture of what they want. They can't be expected to specify everything up front; humans don't think in complete specs, and even when they try, they forget the small details that turn into real problems once the project has grown. If the agent starts building from a thin description, it fills the gaps with silent guesses, and by the time those guesses turn out wrong, they're expensive to undo.
1. **Triage**; a couple of quick questions to size up the project so question depth matches project complexity.
2. **Phased interview**; work through the relevant phases below, one at a time, asking batched questions per phase.
3. **Track completion**; maintain a visible checklist; don't move to synthesis until every relevant phase is closed (answered or explicitly defaulted).
4. **Synthesize**; write the final `prompt.md`. Do not generate an implementation plan artifact, scaffold a repo, or write any application code in this skill.
5. **Hand off**; tell the user to start a new chat, tag `prompt.md`, and ask the agent to execute it.
Never skip straight to building because the request "sounded simple." Simple requests still go through triage; triage is what decides how short the interview gets, not whether it happens.
- Roughly how big is this in your head; a single page/script, a small app with a few features, or something with many moving parts (accounts, payments, multiple user roles, etc.)?
Work through these one phase at a time. Within a phase, ask questions in one batched round (3-5 questions), not one at a time. Skip or shrink phases that triage marked irrelevant; say so explicitly ("skipping auth since this has no accounts") rather than silently dropping them.
### Phase 1: Purpose & users
- What does success look like; what would make you say "yes, this is exactly what I wanted"?
### Phase 2: Core features & flows
### Phase 3: Data & content model
### Phase 4: Tech stack & environment
- Where will this run; a specific hosting platform, local-only, mobile, desktop, browser?
### Phase 5: Integrations & auth
*(skip entirely if triage shows no accounts/external services needed; state that explicitly instead of asking)*
### Phase 6: Non-functional requirements
- Roughly how many people might use this at once; a handful, hundreds, way more?
- Any hard constraints; must work offline, must load instantly, must work on old phones, etc.?
### Phase 7: Edge cases & error states
- What should happen when something goes wrong; bad input, lost connection, empty states (e.g. no data yet)?
### Phase 8: Definition of done
[x] Purpose & users; confirmed
[x] Core features & flows; confirmed
[~] Data & content model; defaulted (assumed simple per-user storage, no sharing)
[ ] Tech stack & environment; open
[-] Integrations & auth; skipped (no accounts needed)
specification; everything needed to build it correctly is below.
<or "None; no accounts or external services required">
<a short, sensible milestone sequence; not a full implementation plan>
Keep it tight and complete rather than padded; every section should contain real decisions, not filler. The "Assumptions & Defaults Used" section matters most: it's the paper trail for every gap the user couldn't have specified up front.
> Your project spec is saved as `prompt.md`. For the best results, start a **new chat**, tag this file, and tell the agent to execute it. Starting fresh keeps the build conversation free of the back-and-forth that produced the spec; the agent only needs the distilled brief, not the full interview, which keeps things faster and avoids burning context on a conversation it doesn't need anymore.