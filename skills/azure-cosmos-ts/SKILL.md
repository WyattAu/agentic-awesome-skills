Data plane SDK for Azure Cosmos DB NoSQL API operations; CRUD on documents, queries, bulk operations.
1. **Use AAD authentication**; Prefer `DefaultAzureCredential` over keys
2. **Always use parameterized queries**; Prevents injection, improves plan caching
3. **Specify partition key**; Avoid cross-partition queries when possible
4. **Use bulk operations**; For multiple writes, use `executeBulkOperations`
5. **Handle 429 errors**; Implement retry logic with exponential backoff
6. **Use ETags for concurrency**; Prevent lost updates in concurrent scenarios
7. **Close client on shutdown**; Call `client.dispose()` in cleanup