Simplify code by reducing complexity while preserving exact behavior. The goal is not fewer lines; it's code that is easier to read, understand, modify, and debug. Every simplification must pass a simple test: "Would a new team member understand this faster than the original?"
- Code is already clean and readable; don't simplify for the sake of it
- You don't understand what the code does yet; comprehend before you simplify
- You're about to rewrite the module entirely; simplifying throwaway code wastes effort
Don't change what the code does; only how it expresses it. All inputs, outputs, side effects, error behavior, and edge cases must remain identical. If you're not sure a simplification preserves behavior, don't make it.
Simplification that breaks project consistency is not simplification; it's churn.
- **Inlining too aggressively**; removing a helper that gave a concept a name makes the call site harder to read
- **Combining unrelated logic**; two simple functions merged into one complex function is not simpler
- **Removing "unnecessary" abstraction**; some abstractions exist for extensibility or testability, not complexity
- **Optimizing for line count**; fewer lines is not the goal; easier comprehension is
Scan for these patterns; each one is a concrete signal, not a vague smell:
| Comments explaining "what" | `// increment counter` above `count++` | Delete the comment; the code is clear enough |
| Comments explaining "why" | `// Retry because the API is flaky under load` | Keep these; they carry intent the code can't express |
Make one simplification at a time. Run tests after each change. **Submit refactoring changes separately from feature or bug fix changes.** A PR that refactors and adds a feature is two PRs; split them.
// Before; consider whether context or composition solves this better.
// This is a judgment call; flag it, don't auto-refactor.
| "The original author must have had a reason" | Maybe. Check git blame; apply Chesterton's Fence. But accumulated complexity often has no reason; it's just the residue of iteration under pressure. |
- [ ] The diff is clean; no unrelated changes mixed in