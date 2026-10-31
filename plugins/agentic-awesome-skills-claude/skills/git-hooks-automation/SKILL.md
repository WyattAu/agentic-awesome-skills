Automate code quality enforcement at the Git level. Set up hooks that lint, format, test, and validate before commits and pushes ever reach your CI pipeline; catching issues in seconds instead of minutes.
Git hooks are scripts that run automatically at specific points in the Git workflow. They live in `.git/hooks/` and are not version-controlled by default; which is why tools like Husky exist.
# The init command creates a pre-commit hook: edit it:
# .githooks/pre-commit: Team-shared hooks directory
> **Warning**: Bypassing hooks should be rare. If your team frequently bypasses, the hooks are too slow or too strict; fix them.
# 5. Clean up: old Husky used package.json config,
- **Staged files only**; Never lint the entire codebase on every commit
- **Auto-fix when possible**; `--fix` flags reduce developer friction
- **Fast hooks**; Pre-commit should complete in < 5 seconds
- **Fail loud**; Clear error messages with actionable fixes
- **Team-shared**; Use Husky or `core.hooksPath` so hooks are version-controlled
- **CI as backup**; Hooks are convenience; CI is the enforcer
- **Gradual adoption**; Start with formatting, add linting, then testing