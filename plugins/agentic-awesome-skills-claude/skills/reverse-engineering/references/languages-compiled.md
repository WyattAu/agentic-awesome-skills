**In Ghidra/IDA:** When you see a function taking `(ptr, int64)`; it's likely a Go string. Three-field `(ptr, int64, int64)` is a slice.
- Deferred calls execute in LIFO order; relevant for cleanup/crypto key wiping
**Key insight:** Go's runtime embeds extensive metadata even in stripped binaries. Use GoReSym before any manual analysis; it often recovers 90%+ of function names, making decompilation dramatically easier. Go strings are `{ptr, len}` tuples, not null-terminated; Ghidra's default string analysis will miss them without the golang-loader plugin.
- Mangled symbols starting with `_ZN` (Itanium ABI); e.g., `_ZN4main4main17h...`
# String (owned): {ptr, capacity, length}: 24 bytes, heap-allocated
# &str (borrowed): {ptr, length}: 16 bytes, can point anywhere
**Key insight:** Rust panic messages are goldmines; they contain source file paths, line numbers, and descriptive error strings even in release builds. Always `strings binary | grep "panicked"` first. Rust's monomorphization means generic functions get duplicated per type; expect many similar-looking functions.
# Much harder to reverse: no reflection metadata
**Key insight:** D binaries are rare in CTFs but identifiable by `_D` symbol prefixes and Phobos library references. The compile-time template system means D functions may be duplicated hundreds of times with different parameters; look for patterns like `enc!("N")` where N varies.