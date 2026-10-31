description: "Send iMessage and SMS from the shell via the @sendblue/cli npm package; outbound sends, contact management, and account setup with no API client or webhook server required."
`@sendblue/cli` is a Node CLI that creates a Sendblue account, provisions an iMessage-enabled number, and sends messages. It is the fastest way to text from a shell, script, or Claude Code hook; no API client, no webhook server, no credentials in env vars. Credentials live at `~/.sendblue/credentials.json` (mode `600`) and Node.js 18+ is required.
`sendblue setup` runs interactively by default. For CI/scripts, run it in two phases; the first call sends an 8-digit verification code by email, the second consumes it.
To text yourself at the end of every agent turn, register a `Stop` hook in `settings.json` that shells out to `sendblue send`. Defer the actual hook wiring to [[update-config]] and the trigger logic to [[sendblue-notify]]; this skill only owns the CLI invocation.
- ❌ **Don't `sudo`**; it writes creds to root's home and the next non-sudo run won't see them.
- Credentials are written to `~/.sendblue/credentials.json` with mode `600`. Treat that file like an API key; do not commit it, do not copy it across machines without the same posture.
- Outbound messages to phone numbers are not free of consequence; wire `sendblue send` into hooks or loops only after gating on duration or success conditions to avoid spamming the recipient.
- **E.164 only.** `5551234567` or `(555) 123-4567` will fail; always `+15551234567`.
- **Free-plan unverified contacts.** Outbound to a contact that hasn't texted in first returns an error; have them text your Sendblue number once, then confirm with `sendblue contacts`.
- **Credentials are per-user.** `~/.sendblue/credentials.json` is owner-only (`600`). Don't `sudo` and pollute root's home; re-running as the same user that ran `setup` is what works.
- `@sendblue-api`; HTTP/JSON alternative for application code, webhooks, and features the CLI does not expose.
- `@sendblue-notify`; Patterns and copy rules for "text me when X is done" workflows that sit on top of this CLI.
- `@update-config`; Wires `sendblue send` into Claude Code hooks (`Stop`, `Notification`) without owning the message logic.