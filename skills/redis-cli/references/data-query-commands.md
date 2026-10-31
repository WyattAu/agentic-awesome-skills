Hashes map string fields to string values; ideal for representing objects. Compact hash encoding is version-dependent; modern versions use listpack rather than legacy ziplist.
- `SDIFF` computes difference starting from the first key; order matters
- `NX`; set expiry only if key has no expiry
- `XX`; set expiry only if key already has an expiry
- `GT`; set expiry only if new TTL is greater than current
- `LT`; set expiry only if new TTL is less than current
- Redis transactions are not rollback-capable; if one command fails, the rest still execute