Admin guide for Apple's `container` CLI runtime (Apple silicon macOS). This is **not Docker** -
`--debug` (env `CONTAINER_DEBUG`). Most `list`/`status` commands accept `--format`; commonly
  and reported. `container system property` has only the `list` subcommand; there is **no**
the builder/vminit image IDs; see table).
|; | `[network] subnetv6` | CIDRv6? | unset | Default IPv6 prefix for networks; auto-allocates when unset (§7) |
|; | `[plugin.<id>]` |; |; | Plugin-scoped config; each plugin reads only its own section |
| `--recommended` | Download+install recommended kernel. **Overrides `--arch`/`--binary`/`--tar`** |; |
| `--binary <path>` | Kernel file, or archive-member name when used with `--tar` |; |
| `--tar <path\|url>` | Tar archive (local or remote) containing the kernel |; |
| `--force` | Overwrite an existing kernel of the same name |; |
- Per-machine kernel override is separate; see §7 (`machine set kernel=<path>`).
- Registering a domain is not enough; set `[dns] domain` in `config.toml` to activate it for
**Auth** (`container registry`, alias `r`); credentials are stored in the macOS keychain:
| `logout <registry>` |; | Deletes keychain creds for the resolved host |
**Insecure / HTTP registries**; `--scheme` (default `auto`):
> `registry list` only lists saved logins. There is no `default-registry get/set` subcommand -
| `stop` |; | No-op warning if not running |
> **⚠️ Requires 1.0.0+; the `container machine` group was added in 1.0.0 (not in 0.7.1).**
container); it runs the image's init system, auto-mounts your macOS `$HOME` and username, and
| `list`/`inspect`/`stop`/`delete`(`rm`)/`set-default` | ID optional (uses default) except `set-default`/`delete` |; |
### Custom networks (`container network`, alias `n`): **macOS 26+**
| `--label k=v` / `--option k=v` | Metadata / plugin option (repeatable) |; |
**property IDs** (`build.rosetta`, `dns.domain`, …); view them read-only with
1. User file; `~/.config/container/config.toml`
2. Package file (optional); `<installRoot>/etc/container/config.toml`
### `[build]`: builder VM (runs `container build`)
| `image` | `image.builder` | String | `ghcr.io/apple/container-builder-shim/builder:<tag>` | Reference for the builder image; `<tag>` tracks the bundled `container-builder-shim` version. (Property ID is `image.builder`, not `build.image`; see the naming exception above.) |
### `[container]`: default per-container resources
### `[dns]`: local DNS domain
### `[kernel]`: default guest kernel
### `[network]`: default subnets
| `subnetv6` |; | CIDRv6? | unset | IPv6 CIDR, e.g. `"fd00:abcd::/64"`. Unset → system auto-allocates. |
### `[registry]`: default registry
### `[vminit]`: default vminitd image
### `[plugin.<id>]`: plugin-scoped config
**MemorySize**; a quoted string: numeric prefix + binary unit suffix (case-insensitive). All
**CIDRv4 / CIDRv6**; quoted strings, e.g. `"192.168.100.0/24"` and `"fd00:abcd::/64"`. Invalid
| User config file | `~/.config/container/config.toml` |; |
| Package config file | `<installRoot>/etc/container/config.toml` (e.g. `/usr/local/etc/container/config.toml`) |; |