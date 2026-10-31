# REQUIRED for GCC coroutine support: build will fail without this
# Standalone Asio has no CMake config: use pkg-config or manual path
**Define `BOOST_ERROR_CODE_HEADER_ONLY` in exactly ONE place; prefer CMake** (`target_compile_definitions`, as shown above). Defining it in CMake *and* with a source `#define` triggers `-Wmacro-redefined`. So in source, just include; no `#define`:
// No macros needed; always header-only