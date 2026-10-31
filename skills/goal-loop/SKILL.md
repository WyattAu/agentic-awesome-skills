- An agent with the `/goal` feature; right now: Codex, Claude Code, or Hermes Agent
- **Subscription auth**; API-key auth does **not** work. A pro-tier plan is the realistic minimum for long runs.
1. **Objective**; one sentence, one concrete outcome.
2. **Constraints**; what must NOT change (public API, files, libs, conventions).
3. **Validation command**; the exact shell command that proves progress (`pytest -q`, `pnpm test`, etc.).
4. **Stop condition**; verifiable: "Stop when X passes" OR "when further changes need human/product input."
5. **Documentation**; one sentence instructing the agent to write concise, targeted docs for every change, either creating new `.md` files or updating existing ones.
When the user wants a quick `/goal` instruction, produce a structured markdown block with one line per contract item (proper newlines, not flowing prose). **Do not prefix the output with `/goal`**; the user adds the slash command themselves in the composer. Emit only the contract body. Template:
**Document:** Write concise, targeted documentation for all changes; create new `.md` files or update existing docs as needed.
- **Documentation is mandatory.** Every `/goal` prompt must include a single sentence committing the agent to concise, targeted docs; new `.md` files or focused updates to existing docs.
- **Never instruct the agent to create new ADRs**; ADRs require the user's explicit approval, so goal prompts must not pre-approve or encourage them.
- **4,000-char limit** on the objective. If longer, put detail in a file (`PLAN.md`/`GOAL_BRIEF.md`) and make the goal point to it; keep the goal itself compact.
- Use **literal strings** for paths, commands, issue numbers; exact.
The agent can now write and set its own goal natively (the `create_goal` tool). Instead of crafting the contract yourself, give it your high-level intent and tell it to set the goal: "Inspect this repo, then write yourself a `/goal` with a verifiable stop condition and pursue it." It's the meta-prompting trick done inline; the agent turns your intent into the contract. Still give it the same raw materials (files to read, constraints, the validation command) so the goal it writes is grounded. Add: "ask clarifying questions before committing if the intent is underspecified"; catches ambiguity up front and prevents the self-set goal from drifting.
2. Launch the agent bare (opens the TUI). **Not** exec/headless mode; `/goal` is a TUI slash command only.
Budget-limited state: the agent doesn't stop abruptly; it summarizes, notes what's left, saves state. `/goal resume` works after budget refresh or upgrade.
- **Loose objective:** `/goal pause`, read status, then `/goal <tighter version>`; replaces the contract. Don't pile instructions on a vague goal.
- **Always review the diff** before merging; long autonomy means more code to validate, not less. Human oversight becomes more critical, not optional.
| Stopped with progress summary | Budget-limited; `/goal resume` after refresh, or tighten scope |
| `/goal resume` says no active goal | Terminal state or cleared; start fresh with `/goal <new>` |
| Goal looks active but won't auto-continue | Stuck in Plan mode; plan-only work doesn't trigger continuation. Draft the plan, then switch to Goal execution |