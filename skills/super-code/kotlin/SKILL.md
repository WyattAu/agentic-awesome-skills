**Prefer `let`, `run`, `apply`, `also`, `with` over repeated receiver references; but don't nest more than 2 levels deep.**
// ✅; top-level if only used in this file
// ✅; put display logic on the enum itself
// ✅; use update{} for atomic mutation
// ✅; only remember if computation is expensive or involves object creation
// ✅; pass only what's needed (stability + minimal recomposition)