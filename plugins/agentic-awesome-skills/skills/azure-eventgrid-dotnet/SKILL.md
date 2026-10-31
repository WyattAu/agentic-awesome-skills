1. **Use CloudEvents**; Prefer CloudEvents for new implementations (industry standard)
2. **Batch events**; Send multiple events in one call for efficiency
3. **Use Entra ID**; Prefer managed identity over access keys
4. **Idempotent handlers**; Events may be delivered more than once
5. **Set event TTL**; Configure time-to-live for namespace events
6. **Handle partial failures**; Acknowledge/release events individually
7. **Use dead-letter**; Configure dead-letter for failed events
8. **Validate schemas**; Validate event data before processing