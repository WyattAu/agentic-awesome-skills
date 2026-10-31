1. **Use Managed Identity**; Prefer `DefaultAzureCredential` over secrets
2. **Enable soft-delete**; Protect against accidental deletion
3. **Use HSM-backed keys**; Set `HardwareProtected = true` for sensitive keys
4. **Implement key rotation**; Use automatic rotation policies
5. **Limit key operations**; Only enable required `KeyOperations`
6. **Set expiration dates**; Always set `ExpiresOn` for keys
7. **Use specific versions**; Pin to versions in production
8. **Cache CryptographyClient**; Reuse for multiple operations