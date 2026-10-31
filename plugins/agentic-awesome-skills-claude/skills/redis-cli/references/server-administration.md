# Selectors (Redis 7.0+): independent permission sets
- `numlocal`; required local AOF fsync count (0 = don't wait for local)
- `numreplicas`; required replica AOF fsync count (0 = don't wait for replicas)
- `timeout`; milliseconds (0 = wait forever)
# Synchronous save (blocks the server: avoid in production)