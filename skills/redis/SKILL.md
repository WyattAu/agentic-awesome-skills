# Security: disable dangerous commands in production
# Sets: unique tags
# Sorted sets: leaderboards
KEYS user:*             # avoid in production; use SCAN instead
# redis.conf: save snapshots at intervals
# Terminal 1: subscriber
# Terminal 2: publisher
# Hit ratio = hits / (hits + misses): aim for > 95%
# Monitor all commands in real time (debugging only: impacts performance)