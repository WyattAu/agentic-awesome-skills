1. **Use `DeveloperToolsCredential` for local dev**; automatically picks up Azure CLI
2. **Use `ManagedIdentityCredential` in production**; no secrets to manage
3. **Clone credentials**; credentials are `Arc`-wrapped and cheap to clone
4. **Reuse credential instances**; same credential can be used with multiple clients
5. **Provide the async runtime**; add Tokio with the features required by your application; do not assume a crate-specific `tokio` feature exists