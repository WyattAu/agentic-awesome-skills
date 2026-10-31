# PR Review Guide: Mode 1
| > 300 lines | Full process; note in the Scope line that review is sampled; cover the highest-risk areas rather than every file |
- Flag immediately if the PR changes more than 10 unrelated files; that itself is a
*Scan this first; it is the most visible risk in a diff.*
Guide with numeric thresholds; use those as the primary reference. When a finding sits
- 🔴 Critical; actively breaking velocity or creating production risk *today*
- 🟡 Warning; will if left unaddressed through the next few features
- 🟢 Suggestion; worth fixing when nearby, not urgent
*Run this last. Three signals only; this is not a full Mode 4 review.*
  → 🟡 Warning: Coverage Illusion; new behavior is untested
  → Source: Feathers; Working Effectively with Legacy Code, Ch. 1
  → 🟡 Warning: Mock Abuse; test complexity exceeds behavior complexity
  → Source: Osherove; The Art of Unit Testing, mock usage guidelines
  → 🟢 Suggestion: Test Obscurity; test intent is unclear from the test name or assertions
  → Source: Meszaros; xUnit Test Patterns, Assertion Roulette (p.224)