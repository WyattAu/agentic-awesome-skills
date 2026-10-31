// ✅; use late only when you guarantee initialization before access
// ✅; promote through null checks
// ✅ (using collection-if/for in a different way; but groupBy isn't built-in)
// ✅; collection-if
// ✅; initializing formals
// ✅; extract widget, or use listen + setState for simple cases
// ✅; catch specific types
// ✅; let exceptions propagate, or use sealed Result type
// ✅; extract subtrees into separate widgets or use ValueListenableBuilder
// ✅; named routes or GoRouter