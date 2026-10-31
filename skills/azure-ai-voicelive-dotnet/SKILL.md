1. **Always set both modalities**; Include `Text` and `Audio` for voice assistants
2. **Use `AzureSemanticVadTurnDetection`**; Provides natural conversation flow
3. **Configure appropriate silence duration**; 500ms typical to avoid premature cutoffs
4. **Use `using` statement**; Ensures proper session disposal
5. **Handle all event types**; Check for errors, audio, text, and function calls
6. **Use DefaultAzureCredential**; Never hardcode API keys