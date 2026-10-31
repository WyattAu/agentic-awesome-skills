  dependencies as arguments; that creates a real Pulumi dependency edge instead
SST already defaults functions to a current Node LTS; on those versions you
- **`??=` is correct here**; and it's exactly what SST's own docs show. The
  user choices; rarely what you want.
  defaults; the behavior above) and is also where `args.hook` / `nodejs`
- **Mixed runtimes need no special guard with `??=`**; an explicit
## The esbuild banner shim strip (version-specific: verify before applying)
`requestId: "-"` in the log; it fails before the handler runs). It can be
`import.meta.dirname`; *not* lexical `__filename`/`__dirname` globals in ESM;
**Only add this hook if all three are true**; otherwise you're stripping lines
    ...args.hook, // MUST spread; clobbering args.hook breaks the transform chain
        // Non-node runtimes don't emit bundle.mjs; skip, then chain.
      // Fail LOUD if SST's banner shape changed; a silent miss re-introduces the crash.
- `nodejs.banner = ""` does **not** replace SST's banner; SST *appends* your
(Context7 → `sst`) before adding the hook to a *new* project; but do not assume the error proves this root cause. Test the exact output and dependent code before removing shim bindings.
  *replaces* the resource; see migrations.
- **Heavily comment the *why***; index sparseness, projection choices, stream
- **Derive everything from `$app.stage`**; never hardcode a stage into a name,
   `Queue`, …); highest-level, gives you `link`, sane defaults, and `.nodes`
2. **Raw `aws.*` Pulumi resource**; when the component doesn't expose what you
3. **`aws.cloudcontrol.Resource`**; last resort, only for services with no typed
**1. `link:`; for same-app Lambdas in the Pulumi graph.** Pass resource handles;
The Lambda consumes the link at runtime via SST's `Resource` binding; this is
Use `link` as the default for "this Lambda needs that table/bucket"; it's the
grants:** `link` issues resource-scoped (not action-scoped) IAM; e.g. broad
redundant; but it's legitimate when you need an *extra* action the link's grant
**2. SSM Parameter Store; for consumers *outside* the Pulumi graph.** CI
ARN), don't have it re-read SSM via a same-run data-source lookup; on a *fresh*
## Secrets: use `sst.Secret`, not plaintext env vars
`environment` block or an SSM `String` parameter; SST has a first-class
// infra/secrets.ts; declare (second arg is an optional placeholder default)
do; a raw `aws.*` resource (or an external value) doesn't; until you wrap it in
(Context7); the linkable API has evolved across v4 minors.
not plain strings. The single most common; and most damaging; mistake is
// BAD; `bucket.arn` is Output<string>. A plain JS template literal stringifies
// GOOD; $interpolate (SST's global; === pulumi.interpolate) resolves the
  value, so plain interpolation of `v` is fine; `` bucket.name.apply((n) => `prefix-${n}`) ``
  unresolved Output; prefer `$interpolate`​`` `${logGroup.arn}` `` at top level
- SST's `.nodes.*` proxies are Output-flavored; reach attributes through
   Cloud Control API limitation; useful to know when you're deciding whether to
   change recreates the resource; but that costs a URL/ID rotation *and* a brief
2. **Outputs are stringly-typed**; no `Output<string>` fields. Read one with
Prefer a typed resource the moment one ships upstream; and migrate to it using