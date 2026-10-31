1. **CI platform**; GitHub Actions, GitLab CI, Jenkins, Azure DevOps, CircleCI, Bitbucket?
2. **Collection source**; local file in repo, or Postman API URL?
3. **Environment**; local env file in repo, or env vars injected by CI secrets?
4. **Reporters needed**; JUnit XML (for CI test results panel), HTML report, or both?
6. **Trigger**; on every push, pull request, schedule, or after deploy?
7. **Fail build on test failure?**; almost always yes; confirm
### Secrets: never hardcode credentials
Newman exits with code `1` if any tests fail; this automatically fails the pipeline step. Use `--bail` if you want to stop on the first failure rather than running all tests.