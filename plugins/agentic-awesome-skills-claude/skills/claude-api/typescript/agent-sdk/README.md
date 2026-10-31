# Agent SDK: TypeScript
1. **Restrict available tools**; Use `tools`/deny rules; `allowedTools` grants automatic approval, not exclusivity
2. **Set working directory**; Always specify `cwd` for file operations
3. **Use appropriate permission modes**; Start with `"default"` and only escalate when needed
4. **Handle all message types**; Check for `result` property to get agent output
5. **Limit maxTurns**; Prevent runaway agents with reasonable limits