General programming footguns by language-not limited to cryptography.
**The Problem**: Signed integer overflow is undefined behavior. Compilers assume it never happens and optimize accordingly-including removing overflow checks.
// Common in FFI-audit all unsafe blocks carefully