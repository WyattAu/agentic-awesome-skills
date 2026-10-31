A plain `import` of an infra module under bare `vitest` fails; `new
1. **Source-level assertions** (the house-style default below); load the
2. **Pulumi runtime mocks** (`@pulumi/pulumi/runtime` `setMocks`); actually
   *behavior*, not just text; use it when a module has real branching logic.
repo's `test` script; check `package.json`; the infra suite may be a separate
describe("infra/storage.ts; resource surface", () => {
- **Count occurrences** to pin cardinality; e.g.
- **Write regression guards as negative assertions**; `expect(src).not.toMatch(
- **IAM scoping**; that a permission is scoped to an ARN, not `*`; that
- **Security defaults**; no `public: true` on buckets; no `$LATEST` invocation on
- **Schema shape**; DynamoDB key/GSI/projection/stream choices; these encode
- **Cross-module contracts**; SSM parameter paths and count (other tools depend
- **Runtime/bundle invariants**; that the Node runtime is pinned and (if the
- **Whether the deploy actually works**; that's an integration concern. Use a
  actually allow the call); verify against a real preview deploy.