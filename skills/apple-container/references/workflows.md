# `container`: Task-Oriented Workflows
Copy-pasteable recipes for Apple's `container` CLI (`apple/container`); Linux containers as
> 0.7.1); run `container --help` to confirm your installed surface.
# Anonymous volume (-v /path): NOT auto-removed by --rm; delete it yourself.
container volume rm anon-<id>  # ⚠️ delete the specific volume; do NOT `grep anon | rm`, which
Note: unlike Docker, anonymous volumes do not auto-clean with `--rm`; remove them via `container volume rm`.
# Reach it from another container by IP (requires macOS 26: see note).
Note: container-to-container access over the virtual network requires macOS 26 and **does not work on macOS 15**, where containers are isolated from one another at the network layer (see `concepts.md` §3); a naming scheme cannot restore connectivity. The DNS domain in the next recipe resolves names **host-side only** (host → container); it does not enable container-to-container traffic on macOS 15.
(there is no `property set` command; properties are edited in the config file):
Note: `container system property` only supports `list`/`ls`; set `dns.domain` via `config.toml`, not a CLI setter. A container named `my-web-server` with domain `test` answers at `my-web-server.test`.
Note: networks are mutually isolated; a container on `foo` has no connectivity to containers on `default`.
# ⚠️ `builder delete` discards the BuildKit builder and its layer cache: the next build
> volume, or network on the machine; not just this project's. `container volume prune` and
# --- Targeted (scoped to named resources: safe for cleaning up one workload) ---
# --- Machine-wide (destroys ALL matching resources: use deliberately) ---
/usr/local/bin/uninstall-container.sh -d      # ⚠️ also deletes ALL user data (images, containers, volumes); irreversible
Note: an XPC/connection error may indicate stopped services, component mismatch, permissions or launch failures; inspect status and logs before retrying. Feature gates to keep in mind: container-to-container access and the `container network` group require **macOS 26** (both unavailable on **macOS 15**). The CLI refuses to run under Rosetta; Apple silicon only.