- **Parallel processing**; Multiple consumers process different partitions
- **Ordered delivery**; Events within a partition maintain order
- **Scalability**; More partitions = higher throughput
3. **Scale consumers** with partitions; aim for 1 consumer per partition max
4. **Monitor lag**; track difference between enqueued and processed events
5. **Handle ownership changes**; design for partitions moving between consumers
7. **Start from checkpoint**; let checkpoint store manage starting position
8. **Partition count depends on tier**; Premium/Dedicated can increase but not decrease; repartitioning changes key mapping and may affect ordering