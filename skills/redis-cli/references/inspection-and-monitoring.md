**server**; Redis version, process ID, uptime, architecture, TCP port, config file
**memory**; Used memory, peak memory, fragmentation ratio, total system memory
**keyspace**; Key counts per database (e.g., `db0:keys=1000,expires=50,avg_ttl=3600`)
**clients**; Connected clients, blocked clients, max clients
**replication**; Role (master/replica), connected replicas, replication offset
**stats**; Total connections, commands processed, keyspace hits/misses
**persistence**; RDB/AOF status, last save time, current save progress
**cpu**; User/system CPU time consumed by Redis
- **WARNING**: This test uses pipelining and stresses the server; never use on production instances
# 1) id           : unique entry ID
# 2) timestamp    : Unix timestamp
# 3) duration     : execution time in microseconds
# 4) command      : array: [cmd, arg1, arg2, ...]
# 5) client       : client address:port
# 6) client_name  : client name (via CLIENT SETNAME)