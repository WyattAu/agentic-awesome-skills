Quick decision guide for choosing the right container base image; balancing security, compatibility, size, and debuggability.
| Go / Rust; fully static binary | `scratch` | `gcr.io/distroless/static-debian12` |
| Go / Rust; with CGO or dynamic libs | `gcr.io/distroless/base-debian12` | `alpine:3.20` |
- ❌ No libc, no shell, no CA certs, no timezone data; must `COPY` them in
- ✅ No shell, no package manager; dramatically reduced attack surface
- ✅ Based on Debian; glibc compatibility (no musl issues)
- ❌ Cannot exec into with `docker exec -it` (no shell); use `:debug` variant for staging
gcr.io/distroless/static-debian12       # No libc; for fully static binaries
gcr.io/distroless/base-debian12         # glibc + openssl; for dynamic Go/Rust
# Debug variants: include busybox shell for staging only
- ✅ Has shell (ash) and package manager (apk); great for debugging
- ⚠️ Uses **musl libc**; some Python C extensions, Node.js native modules, or glibc-dependent binaries may fail
- ✅ glibc compatibility; no musl issues
- ❌ Includes compilers, build tools, package managers; not needed at runtime