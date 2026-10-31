> - `azure-monitor-query-logs`; For Log Analytics queries
> - `azure-monitor-query-metrics`; For metrics queries
1. **Use batch queries**; Combine multiple queries into a single request
2. **Set appropriate timeouts**; Long queries may need extended server timeout
3. **Limit result size**; Use `top` or `take` in Kusto queries
4. **Use projections**; Select only needed columns with `project`
5. **Check query status**; Handle PARTIAL_FAILURE results gracefully
6. **Cache deliberately**; Define freshness and tenant/access-scoped cache keys; metrics may update or arrive late
7. **Migrate to new packages**; Plan migration to `azure-monitor-query-logs` and `azure-monitor-query-metrics`