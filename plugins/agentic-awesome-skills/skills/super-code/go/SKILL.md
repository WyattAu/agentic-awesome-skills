// ✅; always handle; only use _ when error is provably irrelevant
// ✅; each :=/:  is fine; this is idiomatic Go. Don't try to "fix" it.
// ✅; use errors.New or fmt.Errorf unless callers need to inspect type
// ✅; append to nil slice is valid Go
// ✅; use errgroup or WaitGroup to track completion
// ✅; for single-result, buffered channel avoids goroutine leak if receiver exits early
// ✅; blocking select unless you genuinely need non-blocking
// ✅; small, composable interfaces
// ✅; value receiver for small immutable types
// ✅; named returns are worth it only for deferred mutation or documentation
// ✅ (Go <1.22; pass as parameter)
| `if err != nil { return err }` repeated 5+ times | acceptable; it's idiomatic Go |