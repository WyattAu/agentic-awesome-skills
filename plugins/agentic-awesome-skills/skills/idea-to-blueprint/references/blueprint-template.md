- Epic headings: `## Epic 03; <Title>` (two-digit number, em dash). Epic 00 is always "Foundation".
- Story headings inside an epic: `### E03-S02; <Title>`.
### 0. Cover and how to use this document (How to use) : 1 page
### 1. Executive summary (Executive summary): ½–1 page
### 2. Idea interpretation and scope (Scope): 1–2 pages
### 3. Research findings (Research): 4–10 pages
### 4. Assumptions and decision records (Assumptions & Decisions): 1–3 pages
4.1 Assumptions table: `# | Assumption | Why | Impact if wrong`. 4.2 Decision records `DR-nn` in the format from `architecture-decisions.md` §9: status, drivers served, context, options, decision, consequences (including what the rejected option would have bought), **compliance** (the automated check that enforces it, or "manual; reviewed against checklist item X"), and **reversibility** (one-way / medium / cheap). One record per decision, short. Covers stack, architecture style, auth model, data model shape, design system, currency, calendar, hosting. A superseded record stays in the document with its status changed and a pointer forward; why the team changed its mind is often worth more than the current answer.
### 5. Personas (Personas): 2–5 pages
### 6. Goals, non-goals and success metrics (Goals & Metrics): 1–2 pages
### 7. User flows (Flows): 3–8 pages
### 8. Information architecture and screen inventory (Screens): 3–8 pages
### 9. Design direction (Design): 2–4 pages
### 10. UX writing guide (UX Writing): 2–3 pages
### 11. Copy tables (Copy): 5–20 pages
### 12. Architecture (Architecture): 6–12 pages
Full procedure in `architecture-decisions.md`. Order matters: drivers, then shape, then technology; never the reverse.
12.1 **Architecture style decision**: 2–3 candidates scored against the ranked drivers (scoring table), the decision, what the runner-up would have bought and at what price, and the **exit triggers**; the observable conditions under which a future session may revisit the shape.
### 13. Tech stack and engineering conventions (Tech Stack): 3–5 pages
13.1 Stack table: every row answers *what* (pinned version), *why* (the deciding reason, tied to an `AD-n` driver or an agent criterion; never "popular" or "modern"), *what lost and what it would have bought*, and *evidence*. A row missing any of the four is unfinished.
### 14. Quality strategy, review and metrics (Quality): 4–8 pages
14.2 **The gate**: every component of `check` with its command and threshold; format, lint, types, tests, coverage floor, schema-drift check, dependency audit and secret scan, architecture fitness functions, budgets, accessibility and locale checks. State the expected runtime and the rule that the gate is never weakened to make a change pass (and if it is, that is a DR).
14.3 **Test strategy**: pyramid shape, the 3–5 named critical e2e paths, and the test-*quality* rules; behaviour not implementation, concrete assertions, red-before-green, determinism, mutation testing on the critical modules where a maintained tool exists (name it, or say none exists). State explicitly that the coverage number is a floor against regression and never a story goal.
14.8 **Definition of green, definition of done**, and the manual verification policy; what a human checks by hand, when, and how it is recorded.
### 15. Edge-case register (Edge Cases): 3–8 pages
### 16. Delivery plan: sprints and epic order (Delivery Plan): 1–2 pages
### 17. Epics and stories (Epics): 20–60 pages
Preceded by one line stating that everything from here is in English for the coding agent. Then `## Epic 00; Foundation` through the last epic, each following the epic template in `stories-ac-tests.md` §5. Epic 00 always contains the repo bootstrap, tooling, `check`, CI, docs files (BLUEPRINT/PROGRESS/DECISIONS/AGENTS/CLAUDE), environment templates, a hello-world vertical slice with one unit, one integration and one e2e smoke test, and the deployment skeleton to staging.
### 18. Session protocol for Claude Code / Codex (Session Protocol): 2–4 pages
### 19. Risks and mitigations (Risks): 1–2 pages
### 20. Open questions (Open Questions): ½–2 pages
### 21. Glossary (Glossary): 1–2 pages
### 22. Sources (Sources): 1–3 pages
### Appendix A: Session prompts (Appendix Session Prompts)
### Appendix B: Files to create in Epic 0 (Appendix Epic 0 Files)
| Layer | Technology | Pinned version | Deciding reason (driver / agent criterion) | Runner-up; why it lost, what it would have bought | Evidence |
3. Section 3 (research); may need two chunks
8. Section 11 (copy tables); one chunk per 3–5 screens
9. Section 12; **two chunks**: 12.0–12.2 (drivers, style decision, module map) first, then 12.3–12.11
13. Section 17; **one chunk per epic**
- Could any architecture driver in 12.0 have changed the style decision in 12.1? If not, the drivers are decoration; rewrite them with real targets.