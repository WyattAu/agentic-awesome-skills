1. **Use Entra ID in production**; Avoid API keys; use `DefaultAzureCredential`
2. **Reuse client instances**; Create once, share across requests
3. **Handle rate limits**; Implement exponential backoff for 429 errors
4. **Stream for long responses**; Use `CompleteChatStreamingAsync` for better UX
5. **Set appropriate timeouts**; Long completions may need extended timeouts
6. **Use structured outputs**; JSON schema ensures consistent response format
7. **Monitor token usage**; Track `completion.Usage` for cost management
8. **Validate tool calls**; Always validate function arguments before execution