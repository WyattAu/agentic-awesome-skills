# HUNT-NOSQLI: NoSQL Injection
- **MongoDB auth bypass**; `{"username": {"$gt": ""}, "password": {"$gt": ""}}` logs in as first user in collection (usually admin)
- **$where JS injection**; if $where is enabled: blind injection → data exfil
- **Redis command injection**; via SSRF or direct TCP, SLAVEOF attacker-ip → config write → webshell
- **Elasticsearch injection**; _search endpoint with Groovy script injection (pre-5.0) → RCE
### Phase 1: Auth Bypass (MongoDB)
# Regex wildcard: match any username
### Phase 2: URL Parameter Injection
### Phase 3: $where Blind Injection (time-based)
### Phase 3b: Syntax injection into a concatenated `$where`/query (string context)
### Phase 4: Data Dump via Regex
### Phase 5: Automation
### Phase 6: Redis via SSRF
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.