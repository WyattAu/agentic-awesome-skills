- **All 8 dimensions**: do not skip any. If data is insufficient, report "N/A; insufficient session data" rather than omitting.
- **Evidence-based suggestions**: when suggesting description rewrites, cite the specific research finding that motivates the change (e.g., "front-load trigger keywords; MCP study shows 3.6x selection rate improvement").
| Session transcripts | `~/.claude/projects/**/*.jsonl` | `~/.codex/sessions/**/*.jsonl` |; |
**You MUST run ALL 8 dimensions.** The baseline behavior without this skill is to skip dimensions 4.2, 4.3, 4.5b, and 4.8. These are the most valuable dimensions; do not skip them.
**This is the highest-value dimension.** For each skill, extract its **capability keywords** (not just trigger keywords; what the skill CAN do). Then scan user messages for tasks that match those capabilities but where the skill was NOT invoked.
For skills with chronic undertriggering (0 triggers across 5+ sessions where relevant tasks appeared), flag as "compounding risk"; undertriggered skills cannot self-improve through usage feedback, causing the gap to widen over time. Recommend immediate description rewrite as P0.
- **Undertrigger detection**: Memento-Skills (arXiv:2603.18743); skills as structured files require accurate routing; unrouted skills cannot self-improve via the read-write learning loop
- **Description quality**: MCP Description Quality (arXiv:2602.18914); well-written descriptions achieve 72% tool selection rate vs. 20% random baseline (3.6x improvement)
- **Information position**: Lost in the Middle (Liu et al., TACL 2024); U-shaped LLM attention curve
- **Format impact**: He et al. (arXiv:2411.10541); format changes alone can cause 9-40% performance variance
- **Instruction compliance**: IFEval (arXiv:2311.07911); LLMs struggle with multi-constraint prompts