# Design Patterns: Use vs Misuse
- **Repeated variation**; Same algorithm or structure with different behavior (e.g., different discount rules) → Strategy or similar.
- **Lifecycle or creation complexity**; Object creation has many variants or steps → Factory, Builder when the duplication or complexity is real.
- **Cross-cutting concern**; Logging, validation, auth at a boundary → Decorator, middleware, or adapter at the edge.
- **Stable abstraction, varying implementation**; You expect to swap implementations (e.g., repository, gateway) → Interface + implementations; dependency injection.
- **Simple, linear code**; No duplication, one reason to change. Adding a Factory or Strategy here adds indirection without benefit.
- **Speculative need**; "We might need to swap implementations later." Prefer YAGNI; add the abstraction when you actually have a second implementation or a second reason to change.
- **Every class is a pattern**; Not every class needs to be behind a Factory or an Interface. Use patterns where they solve a real design problem.
- **Cargo cult**; Using a pattern because "that’s what we do" or "enterprise code has Factories," without a clear design reason. Symptoms: Factory that only calls `new`, Strategy with one implementation and no plan for a second.
- **Overuse**; Pattern names in every class name; layers that only delegate and add no logic; code that is harder to follow than a straightforward version.
- **Misuse**; Wrong pattern for the problem (e.g., Singleton for something that should be testable and replaceable); pattern that hides the real design (e.g., God Object behind a Facade).
Use this reference in review when someone proposes or has added a Factory, Strategy, Repository, or other pattern-ask "what duplication or variation does this solve?" and "is there a second implementation or a second reason to change?"