// ✅; return a reference when the data lives long enough
// ✅; use Borrow trait; HashMap<String, V> accepts &str as key
// ✅; the ? operator
// ✅; use thiserror for library errors, anyhow for application errors
// ✅; use thiserror with #[from] for automatic conversion
// ✅; destructure in pattern
// ✅; explicit variants
// ✅; use Default + builder pattern or #[derive(Default)] with sensible defaults
// ✅; private field, constructor enforces invariant
// ✅; RwLock for read-heavy
// ✅; use rayon for CPU-bound parallel iteration