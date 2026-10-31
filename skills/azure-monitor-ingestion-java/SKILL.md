1. **Batch logs**; Upload in batches rather than one at a time
2. **Use concurrency**; Set `maxConcurrency` for large uploads
3. **Handle partial failures**; Use error consumer to log failed entries
4. **Match DCR schema**; Log entry fields must match DCR transformation expectations
5. **Include TimeGenerated**; Most tables require a timestamp field
6. **Reuse client**; Create once, reuse throughout application
7. **Use async for high throughput**; `LogsIngestionAsyncClient` for reactive patterns