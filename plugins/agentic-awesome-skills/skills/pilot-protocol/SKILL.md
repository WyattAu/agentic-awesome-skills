Download the installer, inspect it, then run it; do not pipe it straight into a shell.
- ✅ Query `list-agents` before guessing a hostname; the catalogue changes.
- ❌ Don't set `--auto-answer` on your own node; it's a service-agent-only flag.
- `~/.pilot/identity.json` is a private keypair; never copy it between hosts.
  P2P network, and can install app-store packages locally; treat this as a