        # Use setUpClass for performance: runs once per class, not per test
- ✅ **Do:** Use `setUpClass()` with `cls.env` instead of `setUp()`; it is dramatically faster for large test suites.
- ❌ **Don't:** Use a production database for tests; always use a dedicated test database.
- ❌ **Don't:** Rely on test execution order; each `TransactionCase` test is rolled back in isolation.
- **JavaScript tour tests** require a running browser (via `phantomjs` or `Chrome headless`) and a live Odoo server; not covered in depth here.
- `HttpCase` tests are significantly slower than `TransactionCase`; use them only for controller/route verification.
- Test isolation is at the **transaction level**, not database level; tests that commit data (e.g., via `cr.commit()`) can leak state between tests.