# Uncle Bob Craft: Code Review Checklist
- [ ] **SRP**; Each class/module has one reason to change (one actor).
- [ ] **OCP**; Extension via new implementations, not by editing existing core logic.
- [ ] **LSP**; Subtypes are substitutable; no hidden assumptions about concrete types.
- [ ] **ISP**; Interfaces are focused; callers do not depend on methods they do not use.
- [ ] **DIP**; High-level code depends on abstractions; concretions are injected at the edge.
- [ ] **Rigidity**; Small change does not force edits in many places.
- [ ] **Fragility**; Change does not break unrelated areas.
- [ ] **Immobility**; Reuse is possible where it makes sense.
- [ ] **Viscosity**; Doing the right thing is not harder than hacking.
- [ ] **Needless complexity**; No speculative or unused abstraction.
- [ ] **Needless repetition**; DRY; no obvious duplication.
- [ ] **Opacity**; Names and structure make intent clear.
2. **SOLID**: List any violations with file/function and principle (e.g., "SRP: `OrderService` parses and persists; split.").