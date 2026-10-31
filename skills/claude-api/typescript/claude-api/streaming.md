# Streaming: TypeScript
1. **Always flush output**; Use `process.stdout.write()` for immediate display
2. **Handle partial responses**; If the stream is interrupted, you may have incomplete content
3. **Track token usage**; The `message_delta` event contains usage information
4. **Use `finalMessage()`**; Get the complete `Anthropic.Message` object even when streaming. Don't wrap `.on()` events in `new Promise()`; `finalMessage()` handles all completion/error/abort states internally
5. **Buffer for web UIs**; Consider buffering a few tokens before rendering to avoid excessive DOM updates
6. **Use `stream.on("text", ...)` for deltas**; The `text` event provides just the delta string, simpler than manually filtering `content_block_delta` events
7. **For agentic loops with streaming**; See the [Streaming Manual Loop](./tool-use.md#streaming-manual-loop) section in tool-use.md for combining `stream()` + `finalMessage()` with a tool-use loop