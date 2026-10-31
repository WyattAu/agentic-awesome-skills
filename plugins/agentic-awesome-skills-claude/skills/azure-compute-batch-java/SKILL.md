1. **Use Entra ID**; Preferred over shared key for authentication
2. **Use management SDK for pools**; `azure-resourcemanager-batch` supports managed identities
3. **Batch task creation**; Use `createTaskCollection` or `createTasks` for multiple tasks
4. **Handle LRO properly**; Pool resize, delete operations are long-running
5. **Monitor task counts**; Use `getJobTaskCounts` to track progress
6. **Set constraints**; Configure `maxWallClockTime` and `maxTaskRetryCount`
7. **Use low-priority nodes**; Cost savings for fault-tolerant workloads
8. **Enable autoscale**; Dynamically adjust pool size based on workload