# Tool Use: TypeScript
- No manual loop; the SDK handles calling tools and feeding results back
  // finalMessage() resolves with the complete Message; no need to
> **Important:** Don't wrap `.on()` events in `new Promise()` to collect the final message; use `stream.finalMessage()` instead. The SDK handles all error/abort/completion states internally.
> **Error handling in the loop:** Use the SDK's typed exceptions (e.g., `Anthropic.RateLimitError`, `Anthropic.APIError`); see [Error Handling](./README.md#error-handling) for examples. Don't check error messages with string matching.
### JSON Outputs (Zod: Recommended)