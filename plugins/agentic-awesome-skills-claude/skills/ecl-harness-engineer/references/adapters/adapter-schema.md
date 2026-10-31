3. **Graceful degradation**: Every field is optional; missing fields fall back to generic behavior
1. **Project-level override**; verified existing consumer configuration and `DEVELOPMENT.md` commands
2. **Adapter defaults**; The matching adapter's `commands` section
3. **Generic fallback**; `generic.md` adapter (discovers from Makefile/README)
4. No changes to core scripts needed; `detect_adapter.py` auto-discovers adapters