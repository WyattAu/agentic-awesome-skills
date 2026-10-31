**MATCH pattern**; Glob filtering is performed by the server after it scans candidates:
**COUNT n**; Hint for number of elements per call (default: 10):
**TYPE type**; Filter by data type (SCAN only, Redis 6.0+):
**NOVALUES**; Return field names only, without values (HSCAN only):
- Elements may be returned **multiple times**; handle deduplication in your application
- Elements added or removed during iteration may or may not appear; undefined behavior