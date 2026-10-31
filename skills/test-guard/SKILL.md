Use this skill when reviewing generated or changed test code before it ships. Activate it reactively after an agent writes, edits, generates, or refactors tests; unit tests, integration tests, e2e tests, or snapshot tests in any framework.
3. If the project calls LLM APIs, uses agent frameworks, or wires up observability/telemetry, also read [references/llm-app-testing.md](references/llm-app-testing.md); it adds three rules specific to LLM applications.
4. If the user explicitly invokes this skill before test writing, apply the rules as you write; don't write violations and then flag them.
Mock only at system boundaries: network and HTTP calls, LLM APIs, databases, filesystem I/O on external files, clock and randomness, third-party SDKs. Never mock internal classes or helper functions to isolate a "unit"; the seams you create hide the integration bugs worth catching.
Tests that reproduce a real production bug are always justified. Reference the incident (date, issue ID, or short description) in the name or a comment, and preserve their behavioral coverage. Consolidation or replacement is acceptable only when equivalent protection is demonstrated and the incident reference remains. They are exempt from Rule 4; their justification is the incident.
Never mock a data model, DTO, entity, or state object. Construct a real instance. Mocking state hides field-name typos and validation errors; exactly the bugs worth catching. If constructing the real object is painful, that is design feedback, not a reason to mock; add a small builder or factory helper.
- **Must fix:** Rules 1, 2, 8; these hide real bugs or make tests brittle
- **Should fix:** Rules 3, 4, 5, 7; these cause bloat and maintenance drag
- **Sacred:** Rule 6; never delete, always allow
- **Worth noting:** Rule 9; test architecture; flag it, but don't block small changes on it
- [references/pytest.md](references/pytest.md); Python/pytest patterns: parametrize, fixtures, mock boundaries, real Pydantic instances
- [references/phpunit.md](references/phpunit.md); PHP/PHPUnit/Pest patterns, including WordPress and WooCommerce test boundaries
- [references/jest.md](references/jest.md); Jest/Vitest patterns: test.each, module mocks, msw, snapshot discipline
- [references/llm-app-testing.md](references/llm-app-testing.md); three extra rules for LLM applications: prompt contracts, observability wiring, agent-flow transitions
- It does not enforce code style; that's the linter's job.
- It does not decide *what* to test; only reviews test design in the requested scope.