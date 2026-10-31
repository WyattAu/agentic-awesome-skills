Client library for Azure Service Bus; enterprise message broker with queues and publish-subscribe topics.
SERVICEBUS_NAMESPACE=<namespace>.servicebus.windows.net # Required; fully qualified namespace
| **Topic**        | Publish-subscribe messaging; one sender, many subscribers      |
| `complete` | Remove message from queue; processing succeeded   |
| `abandon`  | Release lock; message becomes available for retry |
3. **Use `DeveloperToolsCredential`** for local dev, **`ManagedIdentityCredential`** for production; Rust does not provide a single `DefaultAzureCredential` type
4. **Never hardcode credentials**; use environment variables or managed identity
5. **Assign RBAC roles**; ensure the identity has appropriate Service Bus data roles
6. **Always complete messages**; call `complete_message` after processing to remove from queue
7. **Use topics for fan-out**; when multiple consumers need the same messages, use topics with subscriptions
8. **This crate is pre-production**; APIs may change; pin your dependency version with cargo commands in your dependency workflow