### Example 2: Record Rule: Users See Only Their Own Records
- ✅ **Do:** Use `company_ids` (plural) in multi-company rules; it reflects the companies active in the current environment; test company switching.
- ✅ **Do:** Test rules using a non-admin user in debug mode; `sudo()` bypasses all record rules entirely.
- ❌ **Don't:** Use `base.group_system` for module managers; that grants full technical access including server configurations.
- Record rules are **bypassed by `sudo()`**; any code running in superuser context ignores all `ir.rule` entries.
- Does not cover **row-level security via PostgreSQL** (RLS); Odoo manages all security at the ORM layer.