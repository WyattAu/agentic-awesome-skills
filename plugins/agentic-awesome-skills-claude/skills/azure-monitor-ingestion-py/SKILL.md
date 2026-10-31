1. **Log Analytics Workspace**; Target for your logs
2. **Data Collection Endpoint (DCE)**; Ingestion endpoint
3. **Data Collection Rule (DCR)**; Defines schema and destination
4. **Custom Table**; In Log Analytics (created via DCR or manually)
| **DCE** | Data Collection Endpoint; ingestion URL |
| **DCR** | Data Collection Rule; defines schema, transformations, destination |
- `Custom-<TableName>_CL`; For custom tables
- `Microsoft-<TableName>`; For built-in tables
2. **Handle errors gracefully**; use `on_error` callback for partial failures
3. **Include TimeGenerated**; Required field for all logs
4. **Match DCR schema**; Log fields must match DCR column definitions
6. **Batch uploads**; SDK handles batching, but send reasonable chunks
7. **Monitor ingestion**; Check Log Analytics for ingestion status
8. **Use context manager**; Ensures proper client cleanup