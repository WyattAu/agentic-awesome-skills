1. **Use singletons**; Clients, senders, receivers, and processors are thread-safe
2. **Always dispose**; Use `await using` or call `DisposeAsync()`
3. **Dispose order**; Close senders/receivers/processors first, then client
4. **Use DefaultAzureCredential**; Prefer over connection strings for production
5. **Use processors for background work**; Handles lock renewal automatically
6. **Use safe batching**; `CreateMessageBatchAsync()` and `TryAddMessage()`
7. **Handle transient errors**; Use `ServiceBusException.Reason`
8. **Configure transport**; Use `AmqpWebSockets` if ports 5671/5672 are blocked
9. **Set appropriate lock duration**; Default is 30 seconds
10. **Use sessions for ordering**; FIFO within a session