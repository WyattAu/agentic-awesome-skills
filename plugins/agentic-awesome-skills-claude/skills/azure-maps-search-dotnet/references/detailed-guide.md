1. **Use Entra ID for production**; Prefer over subscription keys
2. **Batch operations**; Use batch geocoding for multiple addresses
3. **Cache only as permitted**; Follow current provider licensing/retention rules, use bounded TTL and refresh stale address data
4. **Use appropriate tile sizes**; 256 or 512 pixels based on display
5. **Handle rate limits**; Implement exponential backoff
6. **Use async route matrix**; For large matrix calculations (>100)
7. **Treat ETA as an estimate**; Traffic availability/options depend on travel mode and current API; this does not guarantee arrival time