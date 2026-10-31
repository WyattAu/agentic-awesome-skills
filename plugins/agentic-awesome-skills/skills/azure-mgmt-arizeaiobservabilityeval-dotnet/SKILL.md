1. **Use async methods**; All operations support async/await
2. **Handle long-running operations**; Use `WaitUntil.Completed` or poll manually
3. **Use GetIfExistsAsync**; Avoid exceptions for conditional logic
4. **Implement retry policies**; Configure via `ArmClientOptions`
5. **Use resource identifiers**; For direct resource access without listing
6. **Close clients properly**; Use `using` statements or dispose explicitly