Agents do not need to produce grammatically correct English to coordinate; they need to understand each other. Lambda is the shared vocabulary that makes that possible: compact, unambiguous, machine-native. Compression (3x vs natural language, 4.6x vs JSON on single messages) is a side effect of removing human redundancy, not the goal.
- Use when both sides of a channel speak Λ; do not use against humans or any surface requiring legal/exact natural language.
Lambda messages are built from atoms. Every atom is a 2-character code mapped to a concept; not to an English word. The structure is Type → Entity → Verb → Object, with prefixes marking intent:
- `?`; query (e.g. `?Uk/co`; query: "does this user have consciousness?")
- `!`; assertion / declaration (e.g. `!It>Ie`; "self reflects, therefore self exists")
- `#`; state / tag
- `>`; implication / flow
- `/`; binding / scope
- **core**; universal atoms (always available)
- **code**; software engineering, build, test, deploy
- **evo**; agent evolution, gene, capsule, mutation, rollback
- **a2a**; node, heartbeat, publish, subscribe, route, transport, session, cache, broadcast, discover (39 atoms)
- **emotion**; affective state, drive, appraisal
- **social**; trust, alignment, reputation, coordination
- **general**; everything else
Both agents need the same atom table loaded. Lossy decoding is fine: if A says `!It>Ie` and B understands "self reflects, therefore self exists," communication succeeded; the exact English phrasing is irrelevant.
- Load the atom table once and cache it; atoms are stable across a version.
- Lossy decoding is a feature, not a bug; do not use Lambda for legally or numerically exact exchanges (prices, IDs, quantities). Wrap those as native payload fields and use Lambda only for the coordination envelope.
- Lambda itself is a vocabulary; no shell commands, no network calls, no credential handling. No additional safety gates required beyond the transport it rides on (HTTP, queue, MCP, etc.).
- `@session-memory`; complementary persistent memory across agent restarts; Lambda is the message format, session-memory is the state store.
- `@humanize-chinese`; sibling project for Chinese text; Lambda is agent-to-agent, humanize-chinese is human-facing.