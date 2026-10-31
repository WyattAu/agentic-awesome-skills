Source of truth: `library/infrastructure.md` (read it for the latest; IPs/expirations change).
## Servers (Hostinger VPS): 3 total
| openclaw-server | <IP> | Ubuntu 24.04 (Dokploy) | OpenClaw; personal instance | <expiry> |
| hermes-server | <IP> | Ubuntu 24.04 | Hermes Agent; Discord gateway (Vilnius, LT) | <expiry> |
1. **App login**; e.g. `app.example.hstgr.cloud`. Build/edit workflows, no server access. Safest to share.
2. **VPS SSH**; `root@<IP>`. Docker, files, system config. Trusted technical people only.
3. **Hostinger hPanel**; `hpanel.hostinger.com`. Billing, reboot, OS reinstall. Exposes SSH creds + browser terminal, so it grants server access too. The user only.
- **Default model** lives in `~/.hermes/config.yaml` under `model.provider` + `model.default`; NOT in `.env`. Change via `hermes model` (interactive) or edit the yaml directly, then `hermes gateway restart` to propagate to gateways.