1. **Use DefaultAzureCredential**; Prefer AAD over connection strings/keys
2. **Delete only after successful processing**; Delivery is at least once; make effects idempotent
3. **Handle poison messages**; Application-managed poison queue; send+delete is not atomic and may duplicate
4. **Use appropriate visibility timeout**; Set based on expected processing time
5. **Extend visibility for long tasks**; Update message to prevent timeout
6. **Use JSON for structured data**; Serialize objects to JSON strings
7. **Check dequeueCount**; Detect repeatedly failing messages
8. **Use batch receive**; Receive multiple messages for efficiency