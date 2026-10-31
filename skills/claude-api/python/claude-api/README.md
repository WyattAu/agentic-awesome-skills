# Claude API: Python
Use top-level `cache_control` to automatically cache the last cacheable block in the request; no need to annotate individual content blocks:
The API is stateless; send the full conversation history each time.
> **Version-sensitive compaction example.** Verify the selected model, context limit, trigger and required beta header before enabling server-side history summarization. The API returns a `compaction` block; you must pass it back on subsequent requests; append `response.content`, not just the text.
    # Append full content: compaction blocks must be preserved
| `tool_use` | Claude wants to call a tool; execute it and continue |
| `refusal` | Claude refused for safety reasons; output may not match your schema |
# Automatic caching (simplest: caches the last cacheable block)