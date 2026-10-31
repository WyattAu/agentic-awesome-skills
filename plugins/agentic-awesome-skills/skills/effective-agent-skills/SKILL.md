- **Context efficiency**; instructions load only when relevant
- **Repeatability**; multi-step procedures become auditable workflows
- **Composability**; multiple skills combine at runtime per task
- **Portability**; same files work across vendors and surfaces
Mental model: skills are to LLMs what man pages, runbooks, and team handbooks are to engineers; reference material loaded into working memory only when the task demands it.
## 3. How they work: progressive disclosure
**Level 1; Discovery (~100 tokens per skill, always in context):**
**Level 2; Activation (<5,000 tokens, loaded on match):**
**Level 3; Execution (bounded by host resources, on demand):**
### Pattern A: Capability primitives (tool wrappers)
### Pattern B: Process primitives (cognitive disciplines)
The skill encodes a methodology the agent should follow. Pure prompt engineering; no scripts needed.
## 6. How to write effective skills: do this
**Never summarize the full workflow in the description.** If the description contains a step-by-step summary of *how* the skill works, the agent tends to follow that summary and skip loading the body. Describe *what* and *when*, never *how*. The description answers "should I open this skill now?"; not "what are the steps?"
Link referenced files directly from SKILL.md. Never build chains (SKILL.md → advanced.md → details.md → actual.md); the agent may preview nested files only partially and miss critical instructions. Add a table of contents to any reference file longer than 100 lines.
## 7. What not to do: anti-patterns
- **"Which skill did you use?"**; ask the agent post-task. Fastest routing debug.
Skills compose at runtime; the agent loads multiple skills as needed for a single task. Design for this: