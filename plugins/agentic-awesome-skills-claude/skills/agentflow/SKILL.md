AgentFlow turns your existing Kanban board into a fully autonomous AI development pipeline. Instead of building custom orchestration infrastructure, it treats your project management tool (Asana, GitHub Projects, Linear) as a distributed state machine; tasks move through stages, AI agents read and write state via comments, and humans intervene through the same UI they already use.
Tasks flow through: Backlog, Research, Build, Review, Test, Integrate, Done. Each stage has specific gates. The Kanban board IS the orchestration layer; no separate database, no message queue, no custom infrastructure.
# Terminal 2: Builder
# Terminal 3: Builder
# Terminal 4: Reviewer
# Terminal 5: Tester
- Do: Review LEARNINGS.md periodically; it captures common failure patterns
- Don't: Skip the deterministic quality gates; they catch most issues cheaply
- Don't: Force-push to main; AgentFlow uses protected scoped rollback for recovery