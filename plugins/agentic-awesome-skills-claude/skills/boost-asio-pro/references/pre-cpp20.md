If you can't use C++20 `co_await`, the **same Asio library** (modern Boost or standalone) still works; only the *async style* changes. Compile with C++11 or later. Two pre-C++20 styles:
1. **Completion handlers (callbacks)**; header-only, C++11, no extra dependencies. The recommended baseline.
2. **Stackful coroutines** (`asio::spawn` + `yield_context`); synchronous-looking like `co_await`, but built on Boost.Coroutine/Boost.Context, so it **must be linked** (not header-only); see build note below.
The full-duplex write-queue rule is identical; a strand alone doesn't stop interleaved writes; just expressed with chained handlers. Capture a previously declared `self` (`auto self = shared_from_this();`) in **every** handler to keep the connection alive.
**Lifetime shift:** coroutine *stack locals* become *member variables* in callback style; a header/body buffer must outlive each async op or it dangles. Chain a composed read of the length, then the body:
// members, NOT locals; they must survive until the handler runs
**Recurring side-task** (e.g. push every 250ms) running concurrently with the read loop; there is no detached coroutine to stop, so use a self-rescheduling timer and `cancel()` it to stop:
Stackful `spawn` additionally requires Boost.Coroutine (which uses Boost.Context); **not header-only**:
> Standalone Asio's `spawn` also depends on Boost.Coroutine/Context; it drags Boost into an otherwise Boost-free build. If you want zero Boost, use the **callback** style.
> The 3-arg `spawn(ex, fn, token)` form needs **Boost ≥ 1.80** (older Boost has only `spawn(ex, fn)`). On old distros like Debian bookworm (Boost 1.74), the **callback** style compiles cleanly while stackful `spawn` does not; verified.