Client library for Azure Key Vault Keys; secure storage and management of cryptographic keys.
1. **Use Entra ID auth**; `DeveloperToolsCredential` for dev, `ManagedIdentityCredential` for production
2. **Use HSM keys for sensitive workloads**; hardware-protected keys
3. **Use EC for signing**; more efficient than RSA
4. **Use envelope encryption**; RSA can wrap a symmetric data key; do not encrypt arbitrary bulk data directly with RSA
5. **Backup keys**; for disaster recovery
6. **Enable soft delete**; required for production vaults
7. **Use key rotation**; create new versions periodically
- `Key Vault Crypto User`; use keys for crypto operations
- `Key Vault Crypto Officer`; full CRUD on keys