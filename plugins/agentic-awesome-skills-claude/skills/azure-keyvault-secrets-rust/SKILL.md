Client library for Azure Key Vault Secrets; secure storage for passwords, API keys, and other secrets.
1. **Use Entra ID auth**; `DeveloperToolsCredential` for dev, `ManagedIdentityCredential` for production
2. **Use `into_model()?`**; to deserialize responses
3. **Use `ResourceExt` trait**; for extracting names from IDs
4. **Handle soft delete**; deleted secrets can be recovered within retention period
5. **Set content type**; helps identify secret format
6. **Use tags**; for organizing and filtering secrets
7. **Version secrets**; new values create new versions automatically
- `Key Vault Secrets User`; get and list
- `Key Vault Secrets Officer`; full CRUD