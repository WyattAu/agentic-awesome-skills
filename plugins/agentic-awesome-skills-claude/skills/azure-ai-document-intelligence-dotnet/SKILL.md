2. **Reuse client instances**; clients are thread-safe
3. **Handle long-running operations**; Use `WaitUntil.Completed` for simplicity
4. **Check field confidence**; Always verify `Confidence` property
5. **Use appropriate model**; Prebuilt for common docs, custom for specialized
6. **Use custom subdomain**; Required for Entra ID authentication