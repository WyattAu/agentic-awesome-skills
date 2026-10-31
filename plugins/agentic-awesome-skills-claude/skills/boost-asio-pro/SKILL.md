description: "Use when writing asynchronous C++ networking code with Boost.Asio or standalone Asio; TCP/UDP servers and clients, SSL/TLS, timers, strands, composed async ops. Covers io_context, co_spawn, awaitable, async_read/async_write, asio::spawn, yield_context, and pre-C++20 callback styles."
Asio's API changed shape three times, so the same task has three correct answers depending on the Boost version in front of you. This skill makes the agent establish that version first, then apply the rules that are genuinely easy to get wrong; strand versus write serialization, buffer and connection lifetime, composed reads for framing; and finally check its own output against a list before calling it done.
Determine the Boost (or Asio) version and the C++ standard actually in use; `find_package(Boost)` output, `dpkg -l libboost-dev`, `brew info boost`, `CMAKE_CXX_STANDARD`, or ask. Do not assume the newest.
| ≥ 1.77 | C++20 | Coroutines (`co_await` + `awaitable<T>`); preferred | [references/coroutines.md](references/coroutines.md) |
| ≥ 1.74 | C++11–17 | Completion handlers (callbacks); the portable baseline | [references/pre-cpp20.md](references/pre-cpp20.md) |
| ≥ 1.80 | C++11–17 | Stackful `asio::spawn` + `yield_context` (links Boost.Coroutine; not header-only) | [references/pre-cpp20.md](references/pre-cpp20.md) |
`io_context`, `make_strand`, `bind_executor`, `steady_timer`, `signal_set`, `async_read`/`async_write`/`async_read_until`, buffers and `resolver` are **library** features; identical in the coroutine and callback styles. Only the suspension mechanism differs.
| `any_io_executor` (`strand<any_io_executor>`, `tcp::socket`'s default executor) | **Boost ≥ 1.74**; the floor for the callback style; below it, use legacy `io_context::strand` |
| `io_context`, `make_strand`, `expires_after` | **Boost ≥ 1.66**; below it, classic `io_service` |
Distro floors that bite: **Debian bookworm ships Boost 1.74** (no `awaitable_operators.hpp`; `#include` fails outright), Ubuntu 20.04 ships 1.71 (no `any_io_executor`), Debian 9 ships 1.62.
**Connections must outlive their handlers.** `enable_shared_from_this`, and capture `self` in *every* `co_spawn` / handler; read loop, write loop, and each timer.
**`async_accept(make_strand(...))` changes two things**: it forces an explicit completion token back on the call, and the accepted socket is `basic_stream_socket<tcp, strand<...>>`, not `tcp::socket`. Take it **by value** or with `auto`; binding it to `tcp::socket&` will not compile.
| Assuming a strand prevents interleaved writes | Add a write queue; see Step 3 |
Same author, same API; namespace and includes differ.
- [ ] Every buffer passed to an async op outlives that op; no callback locals, no dangling `string_view`.
- [ ] It compiles. Build it; most of the mistakes above are compile-time, and the version floors are only real once tested.
Three CI-verified implementations of the same full-duplex framed-protocol server, one per style; copy from the one matching Step 1. Paths are relative to this skill directory; if only the skill was installed, they are at https://github.com/alexprivalov/boost-asio-skill/tree/main/examples.
- Upstream `examples/market-data-feed/` (not bundled); C++20 coroutines (Boost 1.77+; verified 1.83–1.90)
- Upstream `examples/market-data-feed-precpp20/` (not bundled); callbacks, C++11-clean (verified Boost 1.74+, incl. Windows/MSVC)
- Upstream `examples/market-data-feed-classic/` (not bundled); classic `io_service` (verified back to Boost 1.62 / Debian 9)
- This skill does not replace compiling and testing against the target toolchain. The version floors it documents are only real once built; build the code.
- Networking code it produces accepts untrusted input. Validate length prefixes before allocating (`std::string body(n, 0)` with an attacker-controlled `n` is a memory-exhaustion vector; cap it), and verify peer certificates when using TLS rather than disabling verification to make a handshake pass.
- `@cpp-pro`; general modern C++ idioms; this skill assumes them and adds the Asio-specific rules.