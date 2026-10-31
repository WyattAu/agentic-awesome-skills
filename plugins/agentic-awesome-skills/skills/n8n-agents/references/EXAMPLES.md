Three illustrative node-object snippets for the shell + core + sub-agent topology. These are **community n8n JSON fragments** to adapt, not full importable exports; credential IDs, workflow IDs, and channel/bot IDs are placeholders. Build with `n8n_update_partial_workflow` (`addNode` + `addConnection` on the `ai_*` outputs), then verify with `n8n_get_workflow` and `validate_workflow`.
- **`maxIterations: 50`**; raised from the low default because this agent chains several tools per turn.
- **`$now.toISODate()`** in the system prompt; no hardcoded date.
- The sub-agent tool's description repeats **"This tool is stateless"**; the router can't rely on shared context.
- **Anti-loop at the trigger**: `options.userIds: ["U00000000BOT"]` is an exclusion list; the bot's own posts never enter the workflow. No separate filter node needed.
- **`blocksUi`** is the `{ "blocks": [...] }` envelope, not the bare array; the bare array fails silently.
- **Cheaper model** (`claude-haiku-4.6`) than the router; a focused single-domain agent doesn't need the orchestrator's model.
- **Stateless contract** restated in the system prompt; matching the tool description on the core side.
- **`maxIterations: 15`**; fine for a focused sub-agent (vs 50 on the broad router).