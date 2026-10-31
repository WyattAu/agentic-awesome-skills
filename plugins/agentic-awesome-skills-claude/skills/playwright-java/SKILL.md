| New project from scratch | Full scaffold; see `config.md` |
    // Declare ALL locators as fields; never inline in action methods
    // Navigation methods return the next Page Object; enables fluent chaining
    // Arrange via API; faster than navigating through UI
- ✅ Use `assertThat(locator)`; it auto-retries until timeout
- ❌ Never use `Thread.sleep()`; replace with `waitFor()` or `waitForResponse()`
- ❌ Never hardcode base URLs; always use `ConfigReader.getBaseUrl()`
  **Solution:** Ensure `tracing().start()` is called before test actions and `tracing().stop()` is in `@AfterEach`; not `@AfterAll`.
  **Solution:** Add the AspectJ agent to `maven-surefire-plugin` `<argLine>` in `pom.xml`; see `config.md` for the exact snippet.
- `@rest-assured-java`; Use for pure API test suites without any UI interaction
- `@selenium-java`; Legacy alternative; prefer Playwright for all new projects
- `@allure-reporting`; Deep-dive into Allure annotations, categories, and history trends
- `@testcontainers-java`; Use alongside this skill when tests need a live database or service
- `@github-actions-ci`; For building complete multi-browser matrix CI pipelines