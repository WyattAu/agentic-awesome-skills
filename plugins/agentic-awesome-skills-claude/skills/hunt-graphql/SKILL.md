GraphQL vulnerabilities are high-value because the attack surface is both broad and deep; a poorly authorized endpoint may expose data models, privilege escalation paths, and cross-API state confusion. Potentially important surfaces include (no payout guarantee):
- **Race conditions between GraphQL mutations and REST endpoints** where state synchronization is non-atomic; severity depends on demonstrated impact and program policy
1. **Discover the endpoint**; spider JS bundles, check `/graphql`, `/api/graphql`, review Burp passive scan hits for `application/json` POST with query fields
2. **Test introspection**; send the full introspection query. Even if blocked, try field-level enumeration:
3. **Map the applicable schema**; use `InQL` (Burp extension) or `graphql-voyager` to visualize relationships. Specifically look for:
4. **Identify REST/GraphQL overlap**; document every resource that can be modified via BOTH REST and GraphQL. These dual-write surfaces are your RC targets.
5. **Test authorization boundaries per mutation**; replay mutations as lower-privilege users. Does the server enforce the same authz as the equivalent REST call?
6. **Hunt cross-API state desync**; find sequences where:
7. **Test for persistent privilege after role/membership changes**; remove a user via REST, then call the corresponding GraphQL mutation for that resource. Query current state via both APIs and compare.
8. **Probe for IDOR in node IDs**; GraphQL global IDs often encode object type + ID. Swap IDs across object boundaries and across account contexts.
9. **Check batch query abuse**; send arrays of operations to bypass rate limiting or amplify enumeration.
10. **Document the exact reproduction chain**; for RC bugs, time-based steps must be reproducible deterministically.
If response returns `"Did you mean: [realFieldName]?"`; schema is enumerable despite introspection being disabled.
# InQL (Burp extension): visualize GraphQL schema and relationships
# Clairvoyance: brute-force field names when introspection is disabled
# GraphQL Cop: scan for common misconfigurations (introspection enabled, no depth limits, etc.)
1. **Dual-write without atomic locking**; developers implement the same resource modification in both REST and GraphQL independently. Neither system is aware the other exists for that resource. State updates aren't serialized or compared.
2. **Inconsistent authorization middleware**; REST endpoints go through one auth layer (e.g., middleware chain), GraphQL resolvers go through a different resolver-level check. The same action, different enforcement.
3. **GraphQL as "new REST" migration**; teams add GraphQL mutations that mirror REST functionality without auditing the permission model. The GraphQL version is less mature and skips checks the REST version accumulated over time.
4. **Introspection left on in production**; default framework settings (Apollo, Graphene) enable introspection in all environments. Developers forget to disable it, treating it as "just documentation."
5. **Node ID trust without re-authorization**; GraphQL global IDs (`base64("ObjectType:123")`) are decoded and trusted without verifying the requesting user has access to that specific object.
6. **Mutation side effects not mirrored**; when a REST action triggers cascading effects (e.g., team removal cascades to permission revocation), the GraphQL equivalent mutation doesn't trigger the same cascades.
- Bypass via field suggestion errors; send invalid field names and parse "did you mean X?" responses
- Test unauthenticated; some mutations don't check session at all
- Test with tokens from *different organizations*; multi-tenant IDOR
   Must be a concrete action: access data they shouldn't see, retain privileges after revocation, modify another user's resources. "The schema is visible" alone is not enough; what does the schema unlock?
**Scenario A; Covert Persistent Admin After Team Removal (GitHub-pattern)**
An attacker who legitimately had admin access to a repository via team membership gets removed by the org admin through the REST API. The attacker, before removal completes, calls the `updateTeamsRepository` GraphQL mutation to re-associate their team with admin permissions. The REST removal and GraphQL re-grant create a desync where the UI shows the team as removed, but the GraphQL state preserves admin-level access. The attacker retains covert write access to the repository for the observed tested interval; potential code-write impact requires separate proof, reading secrets in CI/CD; without appearing in the team's member list. Do not infer persistence through audits without evidence.
**Scenario B; Covert Access via Repo Transfer Race (GitHub-pattern)**
**Scenario C; Introspection as Reconnaissance Prerequisite (Shopify-pattern)**
On a platform where introspection is intentionally enabled (per-program rules), a hunter maps the applicable schema and discovers undocumented mutations for `fulfillmentOrderMove` and `inventoryAdjust` that are not surfaced in public docs. These mutations accept merchant IDs as arguments with no scoping validation visible in the schema. This recon directly enables targeted IDOR testing against merchant-to-merchant data isolation; the introspection itself is zero-severity, but it is the entry point to critical findings.
## Disclosed Report Citations (Backfill +9: 2019-2024)
4. **HackerOne; Confidential user-data exposure via GraphQL `User` type** ([H1 #489146](https://hackerone.com/reports/489146))
5. **HackerOne; `DestroyLlmConversation` mutation IDOR (Copilot pre-release)** ([H1 #2218334](https://hackerone.com/reports/2218334))
6. **Shopify; `BillingDocumentDownload` cross-tenant IDOR** ([H1 #2207248](https://hackerone.com/reports/2207248))
7. **Shopify; Rate-limit bypass via negative cost** ([H1 #481518](https://hackerone.com/reports/481518))
    - Payload: `query { products(first:-100) { … } }`; negative `first` produced a negative query-cost contribution, refilling the leaky-bucket each call
8. **Stripe; Cross-tenant IDOR via `UpdateAtlasApplicationPerson`** ([H1 #1066203](https://hackerone.com/reports/1066203))
    - Payload: `mutation { updateAtlasApplicationPerson(input:{personId:"<victim_person_id>", …}) }`; adding/modifying a co-founder on another merchant's Stripe Atlas application
9. **EXNESS; SSRF in GraphQL `allTicks` query** ([H1 #1864188](https://hackerone.com/reports/1864188))
    - Payload: `query { allTicks(source:"http://169.254.169.254/latest/meta-data/") { … } }`; `source` arg fed into a server-side HTTP client
10. **EXNESS; GraphQL attribute-batching DoS** ([H1 #2293642](https://hackerone.com/reports/2293642))
11. **GitLab; Malicious-runner attach via `runnerUpdate` (CVE-2023-2478)** ([Advisory](https://about.gitlab.com/releases/2023/05/05/critical-security-release-gitlab-15-11-2-released/))
    - Root cause: `runnerUpdate` did not check that the caller had Maintainer on the target project; any user could bind their malicious runner and intercept CI jobs (build secrets, code execution)
12. **AS Watson; Auth bypass via unrestricted `createAdminUser` mutation** ([HackerOne blog](https://www.hackerone.com/blog/how-graphql-bug-resulted-authentication-bypass))
- **`hunt-idor`**; Review object-level authorization on node/global-ID resolvers; their existence does not imply missing scoping. Chain primitive: GraphQL introspection + IDOR (`node()` resolver) → cross-tenant data via base64-decoded type:id replay.
- **`hunt-api-misconfig`**; GraphQL mutations are mass-assignment magnets: clients send full input objects, server merges. Chain primitive: GraphQL mutation + extra fields (`isAdmin:true`, `verified:true`) → mass assignment → role escalation.
- **`hunt-business-logic`**; GraphQL aliases can call the same mutation multiple times; assess actual per-user/operation limits. Chain primitive: aliased mutation + business-logic flaw → coupon redeemed N times in single network round-trip.
- **`hunt-race-condition`**; Separate concurrent HTTP requests may test backend atomicity; top-level mutation aliases execute serially. Chain primitive: GraphQL batch + race → atomic-update missing → double-spend balance.
- **`security-arsenal`**; Load the GraphQL Payload Pack: introspection query, schema-suggestion error probe, alias amplification template, depth-bomb DoS payload, batch-attack template.
- **`triage-validation`**; Apply the Body-Diff Rule: introspection alone is informational; require a concrete cross-tenant read or mutation-with-impact PoC before submitting.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.