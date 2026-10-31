Client library for Azure Cosmos DB NoSQL API; globally distributed, multi-model database.
1. **Always specify partition key**; required for point reads and writes
2. **Use `into_model()?`**; to deserialize responses into your types
3. **Derive `Serialize` and `Deserialize`**; for all document types
4. **Use Entra ID auth**; prefer `DeveloperToolsCredential` over key auth
5. **Reuse client instances**; clients are thread-safe and reusable