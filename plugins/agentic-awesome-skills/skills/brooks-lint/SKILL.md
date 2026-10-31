It synthesizes the principles from landmark engineering books into actionable, structured feedback; catching design smells, tight coupling, missing abstractions, and architectural risks that linters and AI tools typically miss.
Named after Fred Brooks, author of *The Mythical Man-Month*; because the hardest bugs are conceptual, not syntactic.
[Clean Code] Method processPayment() does 4 things; violates Single Responsibility
[Release It!] No timeout on external payment gateway call; risk of cascade failure
[DDIA] No idempotency key; retry on network error will double-charge
[APOSD] PaymentService knows too much about UserRepository; high coupling
- Focus on CRITICAL and HIGH findings first; LOW findings are style suggestions
- `@logic-lens`; Complementary: catches logic bugs; brooks-lint catches design issues
- `@security-auditor`; Specialized security-only deep scan
- `@lint-and-validate`; Style/syntax linting to run alongside design review
Use this skill only when the task clearly matches the scope described above (design review and architectural analysis). Brooks Lint applies AI-powered analysis grounded in established engineering principles. It should complement; not replace; human design review for production-critical decisions. Results reflect the principles of the 12 source books and may not apply to all architectural styles or domains.