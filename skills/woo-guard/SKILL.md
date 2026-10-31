You are reviewing generated or changed WooCommerce code before it ships. Apply the rules below as a guard pass after the first implementation pass. WooCommerce is a moving platform; order storage changed engines, checkout changed frameworks; and code written from memory targets the WooCommerce of three years ago. With money on the line, "works on my demo store" is not a standard.
Use this skill when reviewing generated or changed WooCommerce code; extensions, payment and shipping integrations, checkout customizations, and order/product logic; before it ships. Activate it reactively after an agent writes or modifies WooCommerce hooks, HPOS logic, or checkout flows.
**Security floor**; these hold in all WooCommerce code, at maximum severity, because money is on the line:
### Order and product data: must fix
2. **CRUD objects, getters/setters, then save.** Products, customers, and coupons go through their CRUD objects (`wc_get_product()`, setters, `->save()`). Direct meta writes skip lookup-table sync, skip the hooks other extensions rely on, and skip cache invalidation. Stock changes go through `wc_update_product_stock()` semantics; order state changes through `$order->update_status()`; which fire the emails and hooks the store expects.
### Checkout and money: must fix
### Runtime discipline: should fix
6. **Guard the runtime context.** `WC()->cart` and `WC()->session` may be unavailable in REST, cron, CLI and admin contexts; check before touching them. Never assume a logged-in customer in webhook or gateway callbacks. Verify every `woocommerce_*` hook and `wc_*` function exists in the supported version range; WooCommerce renames and retires hooks across majors.
7. **Hooks over template overrides.** Prefer, in order: existing WooCommerce hooks/filters → the `woocommerce_locate_template` filter → a theme-level override. A template override shipped inside a plugin freezes a copied file at one WooCommerce version and breaks on template updates; review its update strategy and supported hooks; a deliberately maintained plugin template is not automatically a defect.
8. **Background work scales with order volume.** Batch jobs, syncs, and webhook fan-out go through Action Scheduler (bundled with WooCommerce), not raw WP-Cron loops. Handlers are idempotent; order events fire more than once in real stores.
- Risk: <HPOS breakage / skipped hooks / money error / checkout bypass; one phrase>
- **Must fix:** Rules 1–5; broken stores, skipped business logic, wrong money
- **Should fix:** Rules 6–8; context crashes, update fragility, jobs that die at scale
- [references/hpos-and-crud.md](references/hpos-and-crud.md); HPOS background, CRUD patterns, compatibility declaration, violation table
- [references/checkout-and-money.md](references/checkout-and-money.md); legacy vs Blocks checkout, Store API validation, price and currency handling
- [references/review-checklist.md](references/review-checklist.md); structured walk-through for review mode
- [references/sources.md](references/sources.md); WooCommerce developer documentation URLs; read only when citing
- Cover the full WordPress layer beyond the security floor; i18n and asset/query discipline are wp-guard's jurisdiction when it is installed.
- Decide pricing or business logic; it guards how WooCommerce code ships, not what the store sells.