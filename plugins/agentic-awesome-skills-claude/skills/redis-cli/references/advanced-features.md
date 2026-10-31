# Synchronous mode (blocks server: for debugging only)
# 1) "pmessage"   : message type
# 2) "*"          : pattern matched
# 3) "mychannel"  : channel name
# 4) "mymessage"  : message content
Example; `SET mykey myvalue`:
- `CLUSTER SLOTS` is deprecated since Redis 7.0; use `CLUSTER SHARDS` instead