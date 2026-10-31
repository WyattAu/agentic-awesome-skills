1. **Quick Detection + Intent Confirmation**; what exists, what already passes, and what the user wants.
2. **Analysis**; architecture, harness state, environment, and project identity.
3. **Intake Review + Delta Synthesis**; classify small vs structured work, support requirement-first
4. **Creation/Update**; docs, status handoff, linters, ECL/change scripts, environment config, and CI.
5. **Verification + Handoff**; run checks, attribute failures, update STATUS.md, trigger auto-evolve checks, and summarize results.
- [ ] harness/eval/; agent evaluation datasets and runner inputs
- [ ] harness/trace/; execution traces for agent runs
- [ ] harness/state/; executor runtime state
- [ ] harness/checkpoints/; resumable execution checkpoints
- [ ] harness/memory/; long-term agent memory experiments
- [ ] harness/metrics/; execution and quality metrics
- [ ] docs/DEVELOPMENT.md; missing build commands
- [ ] scripts/lint-quality.py; missing 3 packages in layer map
- [x] Makefile; has all required targets
- [x] .github/workflows/ci.yml; properly configured
This is for harness-executor to implement; not ecl-harness-engineer's responsibility.