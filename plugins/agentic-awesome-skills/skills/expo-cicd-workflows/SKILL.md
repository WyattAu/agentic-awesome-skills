1. **JSON Schema**; https://api.expo.dev/v2/workflows/schema
2. **Syntax Documentation**; https://raw.githubusercontent.com/expo/expo/refs/heads/main/docs/pages/eas/workflows/syntax.mdx
3. **Pre-packaged Jobs**; https://raw.githubusercontent.com/expo/expo/refs/heads/main/docs/pages/eas/workflows/pre-packaged-jobs.mdx
- `name`; Display name for the workflow
- `on`; Triggers that start the workflow (at least one required)
- `jobs`; Job definitions (required)
- `defaults`; Shared defaults for all jobs
- `concurrency`; Control parallel workflow runs
- `github.*`; GitHub repository and event information
- `inputs.*`; Values from `workflow_dispatch` inputs
- `needs.*`; Outputs and status from dependent jobs
- `jobs.*`; Job outputs (alternative syntax)
- `steps.*`; Step outputs within custom jobs
- `workflow.*`; Workflow metadata