Write a structured specification before writing any code. The spec is the shared source of truth between you and the human engineer; it defines what we're building, why, and how we'll know it's done. Code without a spec is guessing.
Don't silently fill in ambiguous requirements. The spec's entire purpose is to surface misunderstandings *before* code gets written; assumptions are the most dangerous form of misunderstanding.
1. **Objective**; What are we building and why? Who is the user? What does success look like?
2. **Commands**; Full executable commands with flags, not just tool names.
3. **Project Structure**; Where source code lives, where tests go, where docs belong.
4. **Code Style**; One real code snippet showing your style beats three paragraphs describing it. Include naming conventions, formatting rules, and examples of good output.
5. **Testing Strategy**; What framework, where tests live, coverage expectations, which test levels for which concerns.
6. **Boundaries**; Three-tier system:
[Build, test, lint, dev; full commands]
[How we'll know this is done; specific, testable conditions]
  - Verify: [How to confirm; test command, build, manual check]
- **Update when decisions change**; If you discover the data model needs to change, update the spec first, then implement.
- **Update when scope changes**; Features added or cut should be reflected in the spec.
- **Commit the spec**; The spec belongs in version control alongside the code.
- **Reference the spec in PRs**; Link back to the spec section that each PR implements.