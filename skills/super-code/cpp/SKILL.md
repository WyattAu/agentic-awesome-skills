// ✅; unique_ptr; move when transferring
// ✅; one lookup when the value is needed
// ✅; const ref for read, move for sink
// ✅; don't const things you intend to move
// ✅; just return the local; compiler applies NRVO or implicit move
// ✅; don't templatize unless you need multiple types
**Concepts make template errors readable; prefer them over SFINAE and static_assert.**
// ✅; std::expected (C++23) or exceptions
// ✅; destructors must be noexcept; log/swallow errors
// ✅; use std::future or condition_variable
**Use `std::scoped_lock` over `lock_guard`; it handles multiple mutexes and avoids deadlock.**
| `std::endl` | `'\n'` (endl flushes; slow) |