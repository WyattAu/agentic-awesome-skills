- **`CONTEXT-MAP.md`** at the repo root if it exists; it points at one `CONTEXT.md` per context. Read each one relevant to the topic.
- **`docs/adr/`**; read ADRs that touch the area you're about to work in. In multi-context repos, also check `src/<context>/docs/adr/` for context-scoped decisions.
If the concept you need isn't in the glossary yet, that's a signal; either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).
> _Contradicts ADR-0007 (event-sourced orders); but worth reopening because…_