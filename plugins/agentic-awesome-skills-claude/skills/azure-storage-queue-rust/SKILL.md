Client library for Azure Queue Storage; send, receive, and manage queue messages.
3. **Use `DeveloperToolsCredential`** for local dev, **`ManagedIdentityCredential`** for production; Rust does not provide a single `DefaultAzureCredential` type
4. **Never hardcode credentials**; use environment variables or managed identity
5. **Assign RBAC roles**; ensure appropriate queue data roles for the identity
7. **Delete messages after processing**; use the message ID and pop receipt from `receive_messages`
8. **Reuse clients**; clients are thread-safe; create once, share across tasks