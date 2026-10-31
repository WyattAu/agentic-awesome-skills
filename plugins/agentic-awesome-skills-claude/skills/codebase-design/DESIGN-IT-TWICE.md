When the user wants to explore alternative interfaces for a chosen deepening candidate, use this parallel sub-agent pattern. Based on "Design It Twice" (Ousterhout); your first idea is unlikely to be the best.
Uses the vocabulary in [SKILL.md](SKILL.md); **module**, **interface**, **seam**, **adapter**, **leverage**.
- A rough illustrative code sketch to ground the constraints; not a proposal, just a way to make the constraints concrete
- Agent 1: "Minimize the interface; aim for 1–3 entry points max. Maximise leverage per entry point."
- Agent 2: "Maximise flexibility; support many use cases and extension."
- Agent 3: "Optimise for the most common caller; make the default case trivial."
1. Interface (types, methods, params; plus invariants, ordering, error modes)
5. Trade-offs; where leverage is high, where it's thin
After comparing, give your own recommendation: which design you think is strongest and why. If elements from different designs would combine well, propose a hybrid. Be opinionated; the user wants a strong read, not a menu.