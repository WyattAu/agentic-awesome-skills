1. **Use environment-appropriate credentials**; DefaultAzureCredential is a server pattern; use supported interactive browser auth and never expose service keys
2. **Set both modalities**; Include `["text", "audio"]` for voice assistants
3. **Evaluate VAD**; Select and test a supported mode for the language, latency and noise conditions
4. **Handle all error types**; Connection, auth, and protocol errors
5. **Clean up subscriptions**; Call `subscription.close()` when done
6. **Use appropriate audio format**; PCM16 at 24kHz for best quality