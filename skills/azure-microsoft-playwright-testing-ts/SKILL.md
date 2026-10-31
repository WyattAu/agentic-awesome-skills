1. **Use Entra ID auth**; More secure than access tokens
2. **Provide explicit credential**; Always pass `credential: new DefaultAzureCredential()`
3. **Enable artifacts**; Set `trace: "on-first-retry"`, `video: "retain-on-failure"` in config
4. **Scale workers**; Use `--workers=20` or higher for parallel execution
5. **Region selection**; Choose region closest to your test targets
6. **HTML reporter first**; When using Azure reporter, list HTML reporter before Azure reporter