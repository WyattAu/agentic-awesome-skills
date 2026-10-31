Client library for Azure Event Hubs; big data streaming platform and event ingestion service.
- **Namespace**; container for Event Hubs
- **Event Hub**; stream of events partitioned for parallel processing
- **Partition**; ordered sequence of events
- **Producer**; sends events to Event Hub
- **Consumer**; receives events from partitions
1. **Reuse clients**; create once, send many events
2. **Use batches**; more efficient than individual sends
3. **Check batch capacity**; `try_add_event_data` returns false when full
4. **Process partitions in parallel**; each partition can be consumed independently
5. **Use consumer groups**; isolate different consuming applications
6. **Handle checkpointing**; use `azure_messaging_eventhubs_checkpointstore_blob` for distributed consumers