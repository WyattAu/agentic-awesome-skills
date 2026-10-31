# Agent SDK: Python
### `query()`: Simple One-Shot Usage
### `ClaudeSDKClient`: Full Control
1. **Restrict available tools**; Use `tools`/deny rules; `allowed_tools` grants automatic approval, not exclusivity
2. **Set working directory**; Always specify `cwd` for file operations
3. **Use appropriate permission modes**; Start with `"default"` and only escalate when needed
4. **Handle all message types**; Check for `ResultMessage` to get agent output
5. **Limit max_turns**; Prevent runaway agents with reasonable limits