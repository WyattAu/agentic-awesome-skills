Every `web_search` call MUST include `workflow: "none"`. This skips the interactive browser curator popup (the user does not want it opening). No exceptions; single query or batched `queries`, always set `workflow: "none"`.
- `web_search`; search the web; returns synthesized answers with citations. Can be called many times per turn. **Always pass `workflow: "none"`.**
- `code_search`; zero-key Exa code-context. Use for library/API/code lookups instead of generic `web_search`.
- `fetch_content`; fetch URL(s) → markdown; handles PDFs, YouTube, GitHub.
- `get_search_content`; big pages (>30k chars) are truncated in responses but stored in full; call this to pull the rest on demand so they don't blow context.
- **GitHub URLs are cloned, not scraped**; you get real files + a local path to explore with `read`/`bash` (private repos need the `gh` CLI). Use this for dev work.
## Routing: match the user's phrasing