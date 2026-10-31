## DRY: Don't Repeat Yourself
### The Rule of 3: wait for the third occurrence
Don't extract an abstraction the first time you see duplication. Don't extract on the second. Wait for the third; by then you have enough signal about the *real* shape of the shared knowledge to abstract correctly. The Rule of 3 is folklore from refactoring practice; the underlying principle (don't abstract too early) is in Fowler's *Refactoring* (refactoring.com/catalog) and in the wrong-abstraction work below.
### The Sandi Metz corollary: wrong abstraction is worse than duplication
## KISS: Keep It Simple, Stupid
**Origin.** Coined by Clarence "Kelly" Johnson at Lockheed's Skunk Works (U-2, SR-71). His designers were handed a basic toolkit; the aircraft had to be repairable by an average mechanic in a combat field with only those tools. The "stupid" refers to the mismatch between break-conditions and repair sophistication; not to the engineer.
- **Cognitive Complexity ≤10 per function.** SonarSource's Cognitive Complexity metric measures *how hard the code is to understand* rather than the count of independent paths. It's the dominant 2026 metric; adopted by SonarQube, Biome, and ReSharper. Target <7 for greenfield code; <15 is the upper bound before refactor is mandatory.
When you see a function exceed cyclo 10 or nest depth 5, refactor *before* finishing; not "later." Extract a helper, replace nested `if/else` with early returns or a lookup table.
## YAGNI: You Aren't Gonna Need It
2. **Cost of delay.** Opportunity cost; revenue-generating work you didn't do instead.
## Ranked list: where AI agents over-engineer
2. **Factory classes for trivial constructors**; `UserFactory.create(...)` wrapping `User(...)`.
3. **DI containers in small apps**; wiring frameworks for 3–5 services where explicit construction in `main()` is shorter and traceable.
4. **Try/catch wrappers that change nothing**; adds lines, hides tracebacks.
5. **Speculative config surface**; settings objects with 15 fields where 3 are read.
7. **`utils.py` / `common.py` modules**; magnets for unrelated functions; violate DRY's "single authoritative representation" by location.
8. **Re-implementing what the platform already gives you**; custom retry loops, enums, or record-like types the standard library provides; hand-rolled validation a database/schema constraint, the type system, or a framework's declarative rule would enforce; a native platform feature replaced by hand-written app code. Prefer the cheapest existing solution: stdlib or native feature over new code, a declarative constraint over an imperative check. The flip side is mode 15 in [ai-failure-modes.md](ai-failure-modes.md); don't reach for a *new* dependency for what a few lines already cover either.
9. **Excessive layering** (Controller → Service → Manager → Repository) for CRUD; four files to read one row.
10. **Wrapping libraries "to make them swappable"**; thin pass-through adapters around an HTTP, database, or SDK client you will never swap.