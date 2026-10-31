# `container`: Concepts & Architecture
> `container images`/`push`/`pull` top-level command); verify in `references/commands.md`.
## 1. What `container` is: one lightweight VM per container
Each per-container VM boots a **minimal Linux**; a small set of core utilities and dynamic
libraries plus a `vminitd` init; rather than a full userland. Consequences of the model:
## 2. Architecture: CLI, API server, and helper services
| `container-runtime-linux` | Container **runtime helper**; **one instance per container**; exposing the management API for that specific container's VM. | One per running container. |
| **macOS** | **macOS 26 is the supported target**; `container` relies on new virtualization/networking features there. It **runs on macOS 15** but with the limitations in §7. Maintainers typically will not address issues that can't be reproduced on macOS 26. |
### macOS 26 vs macOS 15: feature differences
| Container-to-container traffic over the virtual network | Works | **Not possible**; vmnet only creates networks where attached containers are isolated from one another. |
| Multiple / custom networks (`container network` group, `--network <name>`) | Available | **Unavailable**; all containers attach to the single `default` vmnet network; `container network …` and `--network` **error**. |
  network (a direct consequence of the per-container-VM model; there is no shared Docker
  # (there is NO `property set` CLI: only `container system property list` to view values)
  <name> [--subnet … --subnet-v6 …]`. Networks are **mutually isolated**; a container on one
  container's VM; a privacy/isolation win over the shared-VM model where everything must be
  holds `content/`, `containers/`, `images`, `kernels/`, `networks/`, `volumes/`, etc.; see
image and restart the services; see `BUILDING.md` for the exact swift-package steps. Attach a
## 8. Container machines: a related but distinct concept