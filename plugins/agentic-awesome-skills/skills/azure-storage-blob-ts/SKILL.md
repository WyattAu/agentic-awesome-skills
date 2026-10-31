SDK for Azure Blob Storage operations; upload, download, list, and manage blobs and containers.
1. **Use DefaultAzureCredential**; Prefer AAD over connection strings/keys
2. **Use streaming for large files**; `uploadStream`/`downloadToFile` for files > 256MB
3. **Set appropriate content types**; Use `setHTTPHeaders` for correct MIME types
4. **Use SAS tokens for client access**; Generate short-lived tokens for browser uploads
5. **Handle errors gracefully**; Check `RestError.statusCode` for specific handling
6. **Use `*IfNotExists` methods**; For idempotent container/blob creation
7. **Lifecycle**; Use only lifecycle methods actually exposed by the pinned client; do not invent a close() call