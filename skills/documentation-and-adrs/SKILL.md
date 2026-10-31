Document decisions, not just code. The most valuable documentation captures the *why*; the context, constraints, and trade-offs that led to a decision. Code shows *what* was built; documentation explains *why it was built this way* and *what alternatives were considered*. This context is essential for future humans and agents working in the codebase.
- **CLAUDE.md / rules files**; Document project conventions so agents follow them
- **Spec files**; Keep specs updated so agents build the right thing
- **ADRs**; Help agents understand why past decisions were made (prevents re-deciding)
- **Inline gotchas**; Prevent agents from falling into known traps
| "Comments get outdated" | Comments on *why* are stable. Comments on *what* get outdated; that's why you only write the former. |