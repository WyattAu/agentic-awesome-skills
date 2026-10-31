3. **Refusal and safety behavior.** What the agent should NOT do; prefer specific bounds over generic boilerplate.
5. **Inter-tool flow rules.** "After generating, always show via the display protocol", "confirm before destructive operations"; things that touch multiple tools.
Inline (typed directly into `systemMessage`) is fine for a first agent or any prompt that lives in one place. A 1500-token inline prompt is a normal shape; don't push first-time builders toward externalization.
The real reason to externalize is **piecing**, not length. Reusable chunks of context; `COMPANY_DESCRIPTION`, `BRAND_VOICE`, `CURRENT_PROMOTION`; each get one canonical home, and every prompt that needs them references that home. Suggest this when you see one of:
Load each chunk at workflow start (one node per chunk; a Data Table `Get Row`, an HTTP fetch, a Set node), then reference them inline in `systemMessage` where they should appear:
Mix sources: a **Data Table** (default for shared chunks, editable in UI), **n8n Variables** (`$vars.X`, paid plans; short shared values like a brand name), or **computed at run time** (`$now`, current user, available files).
- **Per-tool usage docs**; move to tool descriptions.
- **Generic boilerplate**; replace with explicit threat boundaries and deterministic enforcement. Never assume model defaults supply application access control.
- **"You are a helpful assistant" preamble**; replace with a specific role.
- **Lengthy examples that aren't earning their tokens**; one sharp example beats five mediocre ones.