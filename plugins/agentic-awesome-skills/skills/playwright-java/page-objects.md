// Reusable table component; pass the locator root
Page methods should **return the next page object**; never return `void` for navigating actions:
// CORRECT; enables fluent chaining