# Identity Fabric: Concrete Endpoints
## 22. Identity Fabric: Concrete Endpoints
**getuserrealm.srf; managed vs federated probe:**
**GetCredentialType; user-enum (deep mode only):**
**Reading the result correctly:** `HTTP 200` from these probes is **not sufficient to prove the tenant exists** (Microsoft serves a generic redirect-to-auth page); it does **NOT** mean anonymous access is granted to the tenant's content. Distinguish:
- 404 / NXDOMAIN → tenant not provisioned at this stem (or vanity-named; check known stems from cert transparency).
- **Clairvoyance** (`pip install clairvoyance`); automated field-suggestion enumerator. `clairvoyance -w wordlist.txt -o schema.json https://target.example/graphql`.
- **GraphQL-Cop**; auditor that probes for introspection, batching, depth-limit, suggestion config. `pip install graphql-cop`.
- **InQL** (Burp extension); Burp Suite extension for GraphQL endpoint analysis.
- **GraphQL Voyager**; visualize once schema is reconstructed.