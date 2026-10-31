A confident answer is not a correct one. Long sessions accumulate context that quietly turns assumptions into "facts" without anyone noticing. Doubt-driven development is the discipline of materializing a fresh-context reviewer; biased to **disprove**, not approve; before any non-trivial output stands.
- **Do NOT add this skill to a persona's `skills:` frontmatter.** A persona that follows Step 3 would spawn another persona; the orchestration anti-pattern explicitly forbidden by `references/orchestration-patterns.md` ("personas do not invoke other personas").
- **If you find yourself applying this skill from inside a subagent context** (where Claude Code prevents nested subagent spawn): the preferred path is to surface to the user that doubt-driven cannot run nested and let the main session handle it. As a last resort only, a degraded self-questioning fallback exists; rewrite ARTIFACT + CONTRACT as a fresh self-prompt with a hard mental separator from your prior reasoning, and walk Steps 1–5. This is **not fresh-context review** (you carry your own context with you), so flag the result as degraded and prefer escalation whenever the user is reachable.
- [ ] Step 1: CLAIM; wrote the claim + why-it-matters
- [ ] Step 2: EXTRACT; isolated artifact + contract, stripped reasoning
- [ ] Step 3: DOUBT; invoked fresh-context reviewer with adversarial prompt
- [ ] Step 4: RECONCILE; classified every finding against the artifact text
- [ ] Step 5: STOP; met stop condition (trivial findings, 3 cycles, or user override)
### Step 1: CLAIM: Surface what stands
### Step 2: EXTRACT: Smallest reviewable unit
- Code: the diff or the function; not the whole file
Strip your reasoning. If you hand over conclusions, you'll get back validation of your conclusions. The unit must be small enough that a reviewer can hold it in mind in one read; if it's a 500-line PR, decompose first.
### Step 3: DOUBT: Invoke the fresh-context reviewer
A single-model reviewer shares blind spots with the original author; a different model may reveal other errors, but adds no guarantee of independence or correctness. Doubt-driven is already opt-in for non-trivial decisions, so within that scope offering cross-model is part of the skill's value, not optional friction.
This question is mandatory in every interactive doubt cycle; even on artifacts that feel low-stakes. The user; not the agent; decides whether the cost is worth it. The agent's job is to surface the choice.
**Step 2: If the user picks a CLI; verify, then invoke**
2. Test it works (`gemini --version` or equivalent) before passing the full prompt; a stale or broken binary may pass `which` but fail on real input.
Example shapes (verify flags against your installed tool; syntax differs across implementations and versions):
Surface the failure explicitly. Offer: run it manually, try a different tool, or skip. Do not silently fall back to single-model; the user should know cross-model didn't happen.
- **Never invoke an external CLI without explicit user authorization**; this is a load-bearing safety property.
### Step 4: RECONCILE: Fold findings back
The reviewer's output is data, not verdict. **You are still the orchestrator.** Re-read the artifact text against each finding before classifying; rubber-stamping the reviewer is the same failure mode as ignoring it.
1. **Contract misread**; reviewer flagged something specifically because the CONTRACT you provided was unclear or incomplete. Fix the contract first, re-classify on the next cycle.
2. **Valid + actionable**; real issue requiring a change to the artifact. Change it, re-loop.
3. **Valid trade-off**; issue is real but cost of fixing exceeds cost of accepting. Document the trade-off explicitly so the user sees it.
4. **Noise**; reviewer flagged something that's actually correct under context the reviewer didn't have. Note it, move on, and ask: would adding that context to the contract have prevented the false flag?
### Step 5: STOP: Bounded loop, not recursion
If after 3 cycles the reviewer still surfaces substantive issues, the artifact may not be ready. Surface this to the user; three unresolved cycles is information about the artifact, not a reason to keep looping.
If 3 cycles is "obviously insufficient" because the artifact is large: the artifact is too big; return to Step 2 and decompose. Do not lift the bound.
| "The reviewer disagreed so I was wrong" | The reviewer lacks your context; disagreement is information, not verdict. Re-read the artifact, classify, then decide. |
| "Cross-model is always better" | Cross-model catches blind spots a single model shares with itself, but it adds cost and tool fragility. Offer it every interactive doubt cycle; the user decides whether the artifact warrants it. The agent's job is to surface the choice, not to gate it. |
| "User said yes once, so I can keep invoking the CLI" | Each invocation is its own authorization. The artifact, the prompt, and the flags change between calls; re-confirm the exact command with the user before every run. |
- Doubting only after committing; that's `/review`, not doubt-driven development
- Falling back silently when an external CLI errors or is missing; surface the failure and let the user redirect
- **`test-driven-development`**: TDD's RED step is doubt made concrete; a failing test is a disproof attempt. When TDD applies, that failing test *is* the doubt step for behavioral claims.
- **Repo orchestration rules** (`references/orchestration-patterns.md`): this skill orchestrates from the main session. A persona calling another persona is anti-pattern B; see Loading Constraints above.
- [ ] The reviewer received ARTIFACT + CONTRACT; NOT the CLAIM, NOT your reasoning