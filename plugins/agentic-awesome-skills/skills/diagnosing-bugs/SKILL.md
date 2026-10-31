## Phase 1: Build a feedback loop
**This is the skill.** Everything else is mechanical. If you have a **tight** pass/fail signal for the bug; one that goes red on _this_ bug; you will find the cause; bisection, hypothesis-testing, and instrumentation all just consume it. If you don't have one, no amount of staring at code will save you.
### Ways to construct one: try them in roughly this order
1. **Failing test** at whatever seam reaches the bug; unit, integration, e2e.
4. **Headless browser script** (Playwright / Puppeteer); drives the UI, asserts on DOM/console/network.
A 30-second flaky loop is barely better than no loop; a 2-second deterministic one is tight; a debugging superpower.
The goal is not a clean repro but a **higher reproduction rate**. Loop the trigger 100×, parallelise, add stress, narrow timing windows, inject sleeps. A 50%-flake bug is debuggable; 1% is not; keep raising the rate until it's debuggable.
### Completion criterion: a tight loop that goes red
Phase 1 is done when the loop is **tight** and **red-capable**: you can name **one command**; a script path, a test invocation, a curl; that you have **already run at least once** (paste the invocation and its output), and that is:
- [ ] **Red-capable**; it drives the actual bug code path and asserts the **user's exact symptom**, so it can go red on this bug and green once fixed. Not "runs without erroring"; it must be able to _catch this specific bug_.
- [ ] **Deterministic**; same verdict every run (flaky bugs: a pinned, high reproduction rate, per above).
- [ ] **Fast**; seconds, not minutes.
- [ ] **Agent-runnable**; you can run it unattended; a human in the loop only via `scripts/hitl-loop.template.sh`.
If you catch yourself reading code to build a theory before this command exists, **stop; jumping straight to a hypothesis is the exact failure this skill prevents.** No red-capable command, no Phase 2.
## Phase 2: Reproduce + minimise
Run the loop. Watch it go red; the bug appears.
- [ ] The loop produces the failure mode the **user** described; not a different failure that happens to be nearby. Wrong bug = wrong fix.
Once it's red, shrink the repro to the **smallest scenario that still goes red**. Cut inputs, callers, config, data, and steps **one at a time**, re-running the loop after each cut; keep only what's load-bearing for the failure.
Done when **every remaining element is load-bearing**; removing any one of them makes the loop go green.
## Phase 3: Hypothesise
If you cannot state the prediction, the hypothesis is a vibe; discard or sharpen it.
**Show the ranked list to the user before testing.** They often have domain knowledge that re-ranks instantly ("we just deployed a change to #3"), or know hypotheses they've already ruled out. Cheap checkpoint, big time saver. Don't block on it; proceed with your ranking if the user is AFK.
## Phase 4: Instrument
## Phase 5: Fix + regression test
Write the regression test **before the fix**; but only if there is a **correct seam** for it.
## Phase 6: Cleanup + post-mortem
- [ ] The hypothesis that turned out correct is stated in the commit / PR message; so the next debugger learns
**Then ask: what would have prevented this bug?** If the answer involves architectural change (no good test seam, tangled callers, hidden coupling) hand off to the `/improve-codebase-architecture` skill with the specifics. Make the recommendation **after** the fix is in, not before; you have more information now than when you started.