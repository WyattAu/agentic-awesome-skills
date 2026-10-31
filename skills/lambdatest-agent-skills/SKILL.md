# LambdaTest Agent Skills: Test Automation Registry (46 Skills)
Use the loaded skill context to generate test code that follows real-world conventions; not generic boilerplate. Apply correct import paths, configuration formats, assertion libraries, and runner commands specific to the framework and language.
If the user wants to run tests locally, apply local runner configuration. If running on LambdaTest / TestMu AI cloud, configure RemoteWebDriver capabilities or the appropriate cloud SDK, and set `LT_USERNAME` and `LT_ACCESS_KEY` from environment variables; never hardcode credentials.
- ✅ Always use environment variables for cloud credentials (`LT_USERNAME`, `LT_ACCESS_KEY`); never hardcode them
- ✅ Match dependency versions to what each framework officially recommends; avoid mixing major versions
- ❌ Don't skip writing assertions; a test without assertions is not a test
- ❌ Don't ignore flaky tests; investigate and fix root cause rather than adding retries as a permanent fix
- When generating CI/CD pipelines, store secrets in GitHub Actions Secrets or equivalent; never in plaintext YAML.
  **Solution:** Replace `sleep()` with explicit waits; `waitForSelector` in Playwright, `WebDriverWait` in Selenium, `cy.get().should()` in Cypress.
- `@test-driven-development`; Use when you want to design tests before writing implementation code
- `@testing-patterns`; Use for general testing design patterns and strategies
- `@cicd-pipeline-skill`; Use when setting up end-to-end CI/CD pipelines with test automation
- `@debugging-strategies`; Use when diagnosing systematic test failures