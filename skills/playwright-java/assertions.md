Collect all failures before reporting; critical for form validation tests:
// ❌ WRONG; no auto-retry, brittle
// ✅ CORRECT; auto-retry until timeout
// ❌ WRONG; getText() can return stale value
// ✅ CORRECT; assertion retries until text matches
// ❌ WRONG; count check without waiting
// ✅ CORRECT; waits until count stabilizes