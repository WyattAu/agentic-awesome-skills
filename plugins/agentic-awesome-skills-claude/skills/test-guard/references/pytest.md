# Test Guard: Python / pytest Patterns
- `MagicMock()` standing in for a Pydantic model or dataclass; construct the real thing
# Wrong: hides field typos and validation errors
# Right: Pydantic validates the construction itself
If a model needs many fields, add a factory fixture or use `factory_boy`; don't fall back to `MagicMock`.
- `assert mock.call_count == N` on anything internal; Rule 1 violation
- `@patch` stacks three or more deep; the test is coupled to implementation; restructure
- Asserting incidental log phrasing; avoid it; retain required audit events, redaction and safety signals even when no caller parses them
- Fixtures that build mocks of project classes; Rule 8 violation, make the fixture build real objects