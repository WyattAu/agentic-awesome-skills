description: "Turn a one-line objective into a step-by-step construction plan any coding agent can execute cold. Each step has a self-contained context brief; a fresh agent in a new session can pick up any step without reading prior steps."
# Blueprint: Construction Plan Generator
1. **Research**; Scans the codebase, reads project memory, runs pre-flight checks
2. **Design**; Breaks the objective into one-PR-sized steps, identifies parallelism, records the user’s selected model constraints
3. **Draft**; Generates the plan from a structured template with branch workflow rules, CI policy, and rollback strategies inline
4. **Review**; Reviews the plan against acceptance criteria; delegate only when the user or applicable instructions explicitly authorize sub-agents, and use their model constraints
5. **Register**; Saves the plan/task ledger in an authorized project location; global memory updates require an explicit request
- ✅ Let Blueprint auto-detect git/gh availability; it degrades gracefully