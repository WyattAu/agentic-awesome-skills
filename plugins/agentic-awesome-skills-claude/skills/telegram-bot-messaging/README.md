Pure bash + curl + jq; no install beyond a bot token.
- `TELEGRAM_BOT_TOKEN` / `TELEGRAM_CHAT_ID`; default bot token and default send target.
- `BOT_<NAME>_TOKEN`; a named bot's token, selected with `--bot <name>` (uppercased key, e.g. `--bot alerts` → `BOT_ALERTS_TOKEN`).
- `TARGET_<NAME>=<chat_id>`; a named send target, selected with `--to <name>` (uppercased key, e.g. `--to family` → `TARGET_FAMILY`). A numeric `--to` value is used directly as a chat ID without a lookup.
- `TELEGRAM_APPROVER_IDS`; Telegram user IDs allowed to answer `ask` in a group target.
- `APPROVERS_<NAME>`; target-specific group approvers (for example, `APPROVERS_FAMILY`); overrides the global allowlist for that named target.
- `TELEGRAM_CONFIG_DIR`; overrides the config directory (default `~/.config/telegram`).
- The bot token grants full control of the bot; anyone with it can send/receive as your bot. Treat it like a password.