# Test Guard: PHP / PHPUnit / Pest Patterns
- Mockery/Prophecy doubles for the project's own value objects, DTOs, or entities; construct real instances (Rule 8)
- Mocking internal services just to isolate a class; if wiring is painful, fix the constructor, don't fake the collaborator
- Partial mocks of the class under test; you are no longer testing the class
 * @see Rule 3; variants of one scenario belong in a data provider.
- **Integration tests** (`WP_UnitTestCase` / `wp-env` / `wp-cli scaffold`): use the real WordPress test framework with factories; `self::factory()->post->create()`, `self::factory()->user->create()`. Don't mock `WP_Post` or `WP_User`; the factories exist precisely so you don't have to (Rule 8).
- Build real `WC_Product` / `WC_Order` objects via `WC_Helper_Product` and `WC_Helper_Order` in integration tests; never `MagicMock`-style doubles of them (Rule 8).