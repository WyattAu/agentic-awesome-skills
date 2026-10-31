Client library for Azure Blob Storage; Microsoft's object storage solution for the cloud.
1. **Use Entra ID auth**; `DeveloperToolsCredential` for dev, `ManagedIdentityCredential` for production
2. **Review overwrite behavior**; the reviewed upload API uses options; if_not_exists prevents unintended replacement
3. **Use `RequestContent::from()`**; to wrap upload data
4. **Handle async operations**; use `tokio` runtime
5. **Check RBAC permissions**; ensure "Storage Blob Data Contributor" role
- `Storage Blob Data Reader`; read-only
- `Storage Blob Data Contributor`; read/write
- `Storage Blob Data Owner`; blob-data access and supported ownership/ACL management; it does not grant general Azure RBAC role-assignment authority