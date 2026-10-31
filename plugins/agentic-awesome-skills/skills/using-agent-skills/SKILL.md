Don't silently fill in ambiguous requirements. The most common failure mode is making wrong assumptions and running with them unchecked. Surface uncertainty early; it's cheaper than rework.
- Explain the concrete downside (quantify when possible; "this adds ~200ms latency" not "this might be slower")
Every skill includes a verification step. A task is not complete until verification passes. "Seems right" is never sufficient; there must be evidence (passing tests, build output, runtime data).
2. Not managing your own confusion; plowing ahead when lost