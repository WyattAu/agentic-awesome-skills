The preferred style when the toolchain allows it. Read `SKILL.md` first; the rules there (write queue, buffer lifetime, version floors) apply here and are not repeated.
**Completion tokens:** Control how async results are delivered; `use_awaitable`, `deferred` (default), `detached`, callbacks, futures.
        // Connection closed or error; coroutine ends
**Wrap, don't use bare `as_tuple`.** Always write `as_tuple(use_awaitable)`. Bare `asio::as_tuple` resolves against the operation's *default* completion token (often `deferred`), which compiles in some contexts but fails in others; wrapping an explicit base token is unambiguous everywhere.
- Single-threaded `io_context::run()`; all handlers are sequential
**A strand serializes handler *execution*, NOT whole composed operations.** Two `async_write`s started "concurrently" on the same strand still overlap and **interleave bytes on the wire**; the strand only orders the intermediate handlers, not the byte stream. For full-duplex (a read loop plus pushes/replies writing at the same time on one socket), a strand alone is **not** enough: you must serialize outbound writes yourself with a queue.
// must ALSO pass an explicit completion token; the default-deferred shortcut on
- The accepted socket from `async_accept(make_strand(...))` is `basic_stream_socket<tcp, strand<...>>`, **not** `tcp::socket`. Take it **by value** (`connection(tcp::socket s)`, store `tcp::socket socket_`); the strand executor type-erases into `any_io_executor` on the move. Passing that accepted socket to a `tcp::socket&` (by reference) instead will **fail to compile**; use `auto` or accept by value.
**Strand from inside a coroutine** (when `io` isn't a captured local): get the executor from the coroutine and make a strand off it; no `io_context&` needed:
**Run the read loop and idle watch together**; two `awaitable<void>` branches; don't inspect the result, the first to finish unwinds the other:
    else { /* timeout; cancel the socket */ socket.close(); }
**Re-armable idle timeout** (reset on every received frame; the common server pattern):
// pending wait, resolving the in-flight async_wait with operation_aborted; that
// Wait for both (AND); cancels other on failure
// Wait for first (OR); cancels other on success
**Note:** `||` and `&&` operators require explicit `use_awaitable` token, and the `awaitable_operators.hpp` header (Boost ≥ 1.77; see the version floors in SKILL.md).
**Void branches:** when a branch returns `void` (e.g. two `awaitable<void>` chains), that arm contributes `std::monostate` to the result variant. If *both* branches are void the result is `variant<monostate, monostate>`; don't inspect `.index()`; just `co_await` the expression and let whichever finishes first unwind the other.
For binary protocols, read the fixed-size header fully, then the body fully; two sequential **composed** reads (`async_read` fills the whole buffer, handling short reads). Do NOT use `async_read_some` for framing.