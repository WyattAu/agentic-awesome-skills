Launch **multiple `general` agents in parallel** via the Task tool. Use `general`, not `research`; general agents can spawn their own sub-agents via the Task tool, so when a hunter finds a rabbit hole that needs deeper investigation (e.g., tracing injection into an auth subsystem it doesn't fully understand), it can spin up a focused `research` sub-agent rather than trying to do everything in one context window.
**How many agents?** Use Phase 1 to decide. More focused agents produce better results than broad ones that run out of context. For a small library, 3-4 agents may suffice. For a large application with distinct subsystems, launch 8-12+; split by attack class AND by subsystem. If Phase 1 revealed an auth system, a plugin system, a media pipeline, and a comment engine, each of those could warrant its own injection agent, its own logic agent, etc.
#### Hunting methodology: include in every Phase 2 agent prompt
every layer; from the entry point through validation, transformation, storage, retrieval,
   Data stored then retrieved; is it the same? Does encoding change? Does escaping
about it; extract the suspect function into a minimal harness (or build and run the target)
category; a permission issue while tracing injection, a race condition while reviewing
auth; report it. Don't ignore a bug because it's "not your area." Attackers don't
## Validation rules: apply before reporting ANY finding
3. Check if another layer already prevents exploitation; if so, it's a hardening note, not a finding
5. If your exploit depends on parser/runtime behavior, verify against the relevant spec or implementation; do not reason from intuition.