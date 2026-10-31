  ☑ 3. Take a full backup (database + filestore); your restore point
- ✅ **Do:** Always upgrade on a **copy of production** (staging) first; never the live instance.
- ❌ **Don't:** Upgrade custom modules and Odoo core simultaneously; adapt Odoo core first, then fix custom modules.
- ❌ **Don't:** Run OpenUpgrade against production directly; always test on a staging copy first.