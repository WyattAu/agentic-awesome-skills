# SOLID: the five principles
## S: Single Responsibility Principle
Source: blog.cleancoder.com; SRP, 2014.
## O: Open/Closed Principle
Source: blog.cleancoder.com; OCP, 2014; Martin's 1996 paper PDF (Duke mirror).
- Branch dispatching on a type tag or runtime type check; every new type requires editing the same function.
## L: Liskov Substitution Principle
Substitutability. Callers written against a base type must continue to work when handed a subtype; otherwise polymorphism leaks abstraction.
A caller holding a `Rectangle` reference does `r.set_width(5); r.set_height(4); assert r.area() == 20`. With a `Square`, the assertion fails; LSP violated.
## I: Interface Segregation Principle
*"Make interfaces small."* As a count rule, no. ISP is **client-centric**; segregation is driven by *the set of methods a particular client uses*, not by an arbitrary method-count ceiling. Two clients with identical method needs can share one interface even if it has 20 methods.
## D: Dependency Inversion Principle
- Abstractions defined in the *low-level* package (the interface lives next to its database or service implementation); ownership reversed. The interface should live in the **client's** package.
// billing/charge; high-level policy
// billing/user-repository; abstraction lives WITH the client (billing)
// sql/user-repository; detail depends on the abstraction
1. **God-module** from "do everything in one file" prompts; SRP + DIP + usually OCP.
2. **Type-tag dispatch chains** (`if kind == "pdf": ...`); OCP.
3. **Unsupported-operation stubs in subclasses** when asked to "implement only the methods we need"; LSP + ISP.
4. **Concrete SDK/client imports at module load time**; DIP, hard to test without patching the runtime.
5. **Mega-`Service` interfaces** with create/read/update/delete/email/notify/audit/export; ISP, usually SRP too.
6. **Silent precondition strengthening on override**; defensive-looking, breaks LSP because callers holding the base type now crash on previously-valid inputs.
7. **Invariant-breaking "convenience" subclasses** (e.g., `ReadOnlyList(list)` overriding `append` to no-op); LSP.
8. **Inverted ownership of abstractions**; putting the interface/protocol/abstract contract in the same file as the concrete implementation. Cosmetic DIP fix, real dependency graph unchanged.
5. (DIP) Does the high-level package import the low-level concrete? Where do new abstractions live; with the client or with the implementation?