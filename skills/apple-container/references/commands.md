# `container`: Complete CLI Command Reference
Not Docker. The CLI is deliberately Docker-like, but flags, defaults, and daemon model differ -
**Conventions used below:** a value of `-` in the Default column means "no default (unset /
marks an environment-variable fallback. Command availability varies by macOS version; caveats
0.7.1; namely `container cp`, `container export`, `container prune`, `container image prune`,
Root command: `container`; "A container platform for macOS".
| `--debug` | flag | `false` | Enable debug output. Applies to every command (composed via `Flags.Logging`). `[env: CONTAINER_DEBUG]`; setting the env var (any value) also enables debug. |
| `--version` | flag |; | Print CLI version (`container CLI version …`) and exit. |
| `-h, --help` | flag |; | Show help. On the bare root command, help is augmented with a `PLUGINS:` section. |
and; for the process subset; `exec`. They are defined once here and referenced by name.
| `-e, --env` | `<key=value>` |; | Set env var. `key=value`, or bare `key` to inherit from host. Repeatable. |
| `--env-file` | `<path>` |; | Read env vars from a file (`key=value`; ignores `#` comments and blank lines). Repeatable. |
| `--gid` | `<gid>` |; | Group ID for the process. |
| `-u, --user` | `<user>` |; | User for the process (format `name` \| `uid[:gid]`). |
| `--uid` | `<uid>` |; | User ID for the process. |
| `-w, --workdir, --cwd` | `<dir>` |; | Initial working directory inside the container. (`-w`, `--workdir`, and `--cwd` are all accepted.) |
| `--ulimit` | `<type>=<soft>[:<hard>]` |; | Resource limit (`RLIMIT_*`, e.g. `nofile=1024:2048`). Repeatable. Part of the shared Process group, so it also parses on `exec`. |
| `-c, --cpus` | `<n>` |; | Number of CPUs to allocate. |
| `-m, --memory` | `<size>` |; | Memory (1 MiB granularity); optional `K`/`M`/`G`/`T`/`P` suffix. |
| `--cap-add` | `<cap>` |; | Add a Linux capability (`CAP_NET_RAW`, `NET_RAW`, or `ALL`). Repeatable. |
| `--cap-drop` | `<cap>` |; | Drop a Linux capability. Repeatable. |
| `--dns` | `<ip>` |; | DNS nameserver IP. Repeatable. |
| `--dns-domain` | `<domain>` |; | Default DNS domain. |
| `--dns-option` | `<option>` |; | DNS option. Repeatable. |
| `--dns-search` | `<domain>` |; | DNS search domain. Repeatable. |
| `--entrypoint` | `<cmd>` |; | Override the image entrypoint. |
| `--init-image` | `<image>` |; | Custom init image (customize boot-time behavior: VM daemons, eBPF filters, debugging init). |
| `-k, --kernel` | `<path>` |; | Custom kernel path (resolved to an absolute path). |
| `-l, --label` | `<key=value>` |; | Add a label. Repeatable. |
| `--mount` | `<spec>` |; | Add a mount (`type=<>,source=<>,target=<>,readonly`). Repeatable. |
| `--name` | `<name>` |; | Use this name as the container ID (auto-generated otherwise). |
| `--network` | `<spec>` |; | Attach to a network (`<name>[,mac=XX:XX:XX:XX:XX:XX][,mtu=VALUE]`). Repeatable. |
| `-p, --publish` | `<spec>` |; | Publish a port (`[host-ip:]host-port:container-port[/protocol]`). Repeatable. |
| `--platform` | `<platform>` |; | Platform for multi-platform images (`os/arch[/variant]`). Takes precedence over `--os`/`--arch`. `[env: CONTAINER_DEFAULT_PLATFORM]` |
| `--publish-socket` | `<spec>` |; | Publish a socket (`host_path:container_path`). Repeatable. |
| `--runtime` | `<handler>` |; | Runtime handler (default: `container-runtime-linux`). |
| `--shm-size` | `<size>` |; | Size of `/dev/shm` (e.g. `64M`, `1G`). |
| `--tmpfs` | `<path>` |; | Add a tmpfs mount at the given path. Repeatable. |
| `-v, --volume` | `<spec>` |; | Bind-mount a volume. Repeatable. |
**Arguments:** `<image>` (image name, required) · `<arguments>` (init-process args, captured for passthrough; everything after the image is forwarded verbatim).
**Arguments:** `<container-id>` (required) · `<arguments>` (new process args, passthrough; first arg is the executable).
`-t/--tty`, `-u/--user`, `--uid`, `-w/--workdir/--cwd`, `--ulimit`); `exec` composes the same
| `-s, --signal` | `<signal>` | `SIGTERM` (effective) | Signal to send. The CLI flag itself has no hard default; when omitted the server applies the graceful default (`SIGTERM`). |
Immediately signal (default `KILL`) running containers. No graceful shutdown; use with care.
| `-n` | `<n>` |; (all) | Show the last `n` lines. `-n` is short-only (no `--lines`). Omit to print everything. |
> **⚠️ Requires 1.0.0+** (added after 0.7.1). Removes all *stopped* containers; distinct from
| `--build-arg` | `<key=val>` |; | Build-time variable. Repeatable. |
| `--cache-in` | `<value>` |; | (Hidden/advanced) BuildKit cache import. Repeatable. |
| `--cache-out` | `<value>` |; | (Hidden/advanced) BuildKit cache export. Repeatable. |
| `--dns` | `<ip>` |; | DNS nameserver IP for the build. Repeatable. |
| `--dns-domain` | `<domain>` |; | Default DNS domain. |
| `--dns-option` | `<option>` |; | DNS option. Repeatable. |
| `--dns-search` | `<domain>` |; | DNS search domain. Repeatable. |
| `-l, --label` | `<key=val>` |; | Set a label. Repeatable. |
| `--platform` | `<platform>` |; | Add a platform (`os/arch[/variant]`); precedence over `--os`/`--arch`. `[env: CONTAINER_DEFAULT_PLATFORM]` |
| `--secret` | `id=<key>,...` |; | Build secret (`id=<key>[,env=<ENV_VAR>\|,src=<local/path>]`). Repeatable. |
| `-t, --tag` | `<name>` |; | Name/tag for the built image. Repeatable. |
command; `container images` errors with "plugin not found".)
| `-a, --arch` | `<arch>` |; | Limit to the specified architecture. |
| `--os` | `<os>` |; | Limit to the specified OS. |
| `--platform` | `<platform>` |; | Limit to platform (`os/arch[/variant]`); precedence over `--os`/`--arch`. `[env: CONTAINER_DEFAULT_PLATFORM]` |
| `-a, --arch` | `<arch>` |; | Limit to architecture. |
| `--os` | `<os>` |; | Limit to OS. |
| `--platform` | `<platform>` |; | Limit to platform; precedence over `--os`/`--arch`. `[env: CONTAINER_DEFAULT_PLATFORM]` |
| `-a, --arch` | `<arch>` |; | Architecture for the saved image. |
| `--os` | `<os>` |; | OS for the saved image. |
| `--platform` | `<platform>` |; | Platform (`os/arch[/variant]`); precedence over `--os`/`--arch`. `[env: CONTAINER_DEFAULT_PLATFORM]` |
> elsewhere; that concept lives in system configuration, not this group).
| `--dns` | `<ip>` |; | DNS nameserver IP. Repeatable. |
| `--dns-domain` | `<domain>` |; | Default DNS domain. |
| `--dns-option` | `<option>` |; | DNS option. Repeatable. |
| `--dns-search` | `<domain>` |; | DNS search domain. Repeatable. |
| `--localhost` | `<ip>` |; | IPv4 address to redirect to localhost. |
| `--binary` | `<path>` |; | Path to the kernel file (or archive member if used with `--tar`). |
| `--tar` | `<tar>` |; | Filesystem path or remote URL to a tar archive containing a kernel file. |
| `--label` | `<key=value>` |; | Network metadata. Repeatable. |
| `--option` | `<key=value>` |; | Plugin-specific option. Repeatable. |
| `--subnet` | `<cidr>` |; | IPv4 subnet (e.g. `192.168.100.0/24`). |
| `--subnet-v6` | `<cidr>` |; | IPv6 prefix (e.g. `fd00:1234::/64`). |
| `--label` | `<key=value>` |; | Volume metadata. Repeatable. |
| `--opt` | `<key=value>` |; | Driver-specific option. Repeatable. |
| `-s` | `<size>` |; | Volume size in bytes; optional `K`/`M`/`G`/`T`/`P` suffix. Takes precedence over `--opt size=`. |
- `size=<value>`; volume size (unit suffixes as above; min 1 MiB). Equivalent to `-s`; `-s` wins if both set.
- `journal=<mode>[:<size>]`; ext4 journaling. `<mode>` ∈ `ordered` (metadata only, data-before-metadata; kernel default), `writeback` (metadata only, no data ordering; fastest, least safe), `journal` (metadata + data; safest). Optional `:<size>` sets the journal size.
name `anon-…`). Unlike Docker, they do **not** auto-clean with `--rm`; delete manually.
> **⚠️ Requires 1.0.0+**; the entire `container machine` group was added after 0.7.1. Verify with
| `-n, --name` | `<name>` |; | Name for the machine. |
| `--cpus` | `<n>` |; | Number of virtual CPUs. |
| `--kernel` | `<path>` |; | Path to a custom kernel binary (e.g. `vmlinux`). |
| `--platform` | `<platform>` |; | Platform for a multi-platform image; precedence over `--os`/`--arch`. |
| `-n` | `<n>` |; (all) | Show the last `n` lines. `-n` is short-only. |