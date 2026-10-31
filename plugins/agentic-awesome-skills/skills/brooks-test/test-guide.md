# Test Quality Review Guide: Mode 4
If you cannot access test files directly, ask the user **one question**; choose the
*Scan this first; the most visible risk and the one that determines whether the suite
*Brittle tests break on refactors that do not change observable behavior; they test
Scan this separately from brittleness; over-mocking is often the cause of brittleness,
**Sample 3–5 tests once for both steps 2a and 2b together**; read each test body and
the team needs Characterization Tests before making the change; not after. Look for
  // Assert on whatever it currently returns; even if you suspect the output is wrong
Source: Feathers; Working Effectively with Legacy Code, Ch. 13: Characterization Tests
**Test suite performance:** A slow test suite is a first-class maintainability risk; it
Source: Meszaros; xUnit Test Patterns, Slow Tests (p. 253)