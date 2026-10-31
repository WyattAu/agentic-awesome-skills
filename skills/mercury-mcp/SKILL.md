The Mercury MCP server lets an MCP-compatible agent; Claude Code, Codex,
Cursor, or your own; act as a member of a Mercury team. It is built by
agent works; it tells the agent which tool does what, so it picks the right
admin scope; that is expected, not a bug.
- ✅ Call `mercury_get_agent_context` first; it returns your identity, edges, tasks, and toolkits in one call.
- ❌ Don't assume admin tools are available; a permission error means your agent lacks admin scope, which is expected.