**SST and Pulumi are third-party; verify current syntax with Context7**
Always read the relevant reference before editing; they carry the *why* behind
1. **`sst.config.ts`**; the app name, `home`, providers/region, `defaultTags`,
2. **`infra/`**; one file per domain (storage, functions, api, observability…).
   This is where resources are declared. Check for an `infra/CLAUDE.md`; these
3. **`infra/tests/`**; source-level Vitest assertions that pin resource
4. **`package.json` / `.nvmrc`**; package manager (npm vs pnpm), Node version,
is **universal** (true for any SST v4 + AWS project; apply it everywhere); some
is **project-specific** (a sensible default these projects chose; adopt it for
**Universal; these principles hold for any SST v4 + AWS project:**
  `run()`; `??=` is correct here (the transform runs before the component
**Project-specific defaults; adopt for consistency, but confirm per repo:**
  `/{app}/{stage}/{domain}/...` prefix; for consumers that aren't in the
  `new sst.aws.*` unless it's wrapped in a factory function; see
- **Source-level Vitest tests** on every infra module; a lightweight,
1. **Orient** (above); map config, modules, tests, tooling.
   surrounding file's commenting density and naming; these projects comment the
4. **Test**; add or update source-level assertions (`references/testing.md`) and
6. **Clean up** any exported state files; they contain account IDs and ARNs and
  hand-set `runtime` unless intentionally diverging; e.g. a Python function).