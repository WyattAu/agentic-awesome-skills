- Run the test suite to check for regressions; concurrency changes can introduce subtle runtime issues even when the build is clean.
**UI-bound type; adding `@MainActor`**
// Before: compiler error; SomeProtocol method is nonisolated but the