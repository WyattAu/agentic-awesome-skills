3. **Always use `DefaultAzureCredential`**; never hardcode keys
8. **Enable TLS 1.2 minimum**; set `MinimumTlsVersion = RedisTlsVersion.Tls1_2`
9. **Disable non-SSL port**; set `EnableNonSslPort = false` for security
10. **Rotate keys regularly**; use `RegenerateKeyAsync` and update connection strings
1. **SKU downgrades not allowed**; You cannot downgrade from Premium to Standard/Basic
2. **Clustering requires Premium**; Shard configuration only available on Premium SKU
3. **Geo-replication requires Premium**; Linked servers only work with Premium caches
4. **VNet injection requires Premium**; Virtual network support is Premium-only
5. **Patch schedules require Premium**; Maintenance windows only configurable on Premium
6. **Cache name globally unique**; Redis cache names must be unique across all Azure subscriptions
7. **Long provisioning times**; Cache creation can take 15-20 minutes; use `WaitUntil.Started` for async patterns