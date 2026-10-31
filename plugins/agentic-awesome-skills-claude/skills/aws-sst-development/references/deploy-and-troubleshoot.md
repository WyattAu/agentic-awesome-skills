All via the project's package manager (`npx`, `pnpm exec`, or an npm script -
| `npx sst state ...` | Inspect/repair Pulumi state; see hygiene below. |
1. **Confirm the target account.** `aws sts get-caller-identity`; a deploy goes
   (Pulumi printing `+-` / "replace"); a replacement on a stateful resource
3. **Type-check and test**; `tsc --noEmit` and the infra Vitest suite
  `pulumi import` for state surgery; manual edits corrupt the checkpoint.
**Always delete the exported state file when done**; it contains account IDs and
(Applies only to projects using durable/long-running Lambdas; `durable: true`.)
monitoring *before* merge; cheap insurance against a silent prod regression.
  Lambda code; it bypasses the JSON formatter your metric filters match on.
`on: pull_request: types: [opened, synchronize, reopened, closed]`; note there's