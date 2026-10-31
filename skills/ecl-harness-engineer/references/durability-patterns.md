This reference documents patterns for building durable agent infrastructure; harnesses that survive failures, manage context across long sessions, and maintain agent effectiveness over extended workstreams.
| **Episodic** | What happened | `harness/memory/episodes/`; timestamped event logs | When debugging why something went wrong |
| **Semantic** | What I know | `harness/memory/knowledge/`; facts about this codebase | Before making architectural decisions |
| **Procedural** | How I do things | `harness/memory/procedures/`; learned workflows | When starting familiar task types |
    "chi router middleware order matters; auth before logging",
Real agents don't just respond to prompts; they pursue outcomes. A harness should support goal tracking, progress monitoring, and intelligent escalation.