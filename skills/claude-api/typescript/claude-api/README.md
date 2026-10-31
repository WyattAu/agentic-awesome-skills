# Claude API: TypeScript
Use the SDK's typed exception classes; never check error messages with string matching:
The API is stateless; send the full conversation history each time. Use `Anthropic.MessageParam[]` to type the messages array:
- Use SDK types (`Anthropic.MessageParam`, `Anthropic.Message`, `Anthropic.Tool`, etc.) for all API data structures; don't redefine equivalent interfaces
> **Version-sensitive compaction example.** Verify the selected model, context limit, trigger and required beta header before enabling server-side history summarization. The API returns a `compaction` block; you must pass it back on subsequent requests; append `response.content`, not just the text.
  // Append full content; compaction blocks must be preserved
| `tool_use`      | Claude wants to call a tool; execute it and continue           |
| `refusal`       | Claude refused for safety reasons; output may not match schema |
// Automatic caching (simplest; caches the last cacheable block)