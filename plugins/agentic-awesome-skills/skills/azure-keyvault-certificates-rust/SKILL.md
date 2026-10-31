Client library for Azure Key Vault Certificates; secure storage and management of certificates.
1. **Create**; generates new certificate with policy
2. **Import**; import existing PFX/PEM certificate
3. **Get**; retrieve certificate (public key only)
4. **Update**; modify certificate properties
5. **Delete**; soft delete (recoverable)
6. **Purge**; permanent deletion
1. **Use Entra ID auth**; `DeveloperToolsCredential` for dev
2. **Use managed certificates**; auto-renewal with supported issuers
3. **Set proper validity period**; balance security and maintenance
4. **Use certificate policies**; define renewal and key properties
5. **Monitor expiration**; set up alerts for expiring certificates
6. **Enable soft delete**; required for production vaults
- `Key Vault Certificates Officer`; full CRUD on certificates
- `Key Vault Reader`; read certificate metadata