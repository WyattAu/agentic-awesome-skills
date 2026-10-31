# MockHunter: Live Page Reality Check
- Use when a dashboard "looks too clean"; every metric uniformly round, all timestamps clustered, no variance; and you suspect seeded data
├── YES; found in a response:
└── NO; value not in any network response:
- ✅ Run cold-start tests (zero data); many vibe-coded apps fail there
- ❌ Don't run active interaction on apps you don't own without permission; live clicks and form submissions can mutate state
- ❌ Don't trust a destructive-button exclusion list by itself; localized labels, icons, aria text, and backend routes can hide mutating actions
- ❌ Don't trust the audit if the page failed to load; check console first
- Single-page audit per run; no multi-page crawl in v0.1.0
- Form-login only for auth; no OAuth, magic-link, or 2FA in v0.1.0
- Markdown report only; no JSON output yet
- All Playwright actions happen in a controlled MCP browser context; no headless escalation