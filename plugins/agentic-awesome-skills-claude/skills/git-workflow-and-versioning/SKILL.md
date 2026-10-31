Keep `main` always deployable. Work in short-lived feature branches that merge back within 1-3 days. Long-lived development branches are hidden costs; they diverge, create merge conflicts, and delay integration. DORA research consistently shows trunk-based development correlates with high-performing engineering teams.
This is the recommended default. Teams using gitflow or long-lived branches can adapt the principles (atomic commits, small changes, descriptive messages) to their branching model; the commit discipline matters more than the specific branching strategy.
- `feat`; New feature
- `fix`; Bug fix
- `refactor`; Code change that neither fixes a bug nor adds a feature
- `test`; Adding or updating tests
- `docs`; Documentation only
- `chore`; Tooling, dependencies, config
Don't combine formatting changes with behavior changes. Don't combine refactors with features. Each type of change should be a separate commit; and ideally a separate PR:
**Separate refactoring from feature work.** A refactoring change and a feature change are two different changes; submit them separately. This makes each change easier to review, revert, and understand in history. Small cleanups (renaming a variable) can be included in a feature commit at reviewer discretion.
- Keep branches short-lived (merge within 1-3 days); long-lived branches are hidden costs
- The Zod schema is strict; rejects extra fields. Confirm this is desired.
- Added zod as a dependency (72KB gzipped); already in package.json
This pattern catches wrong assumptions early and gives reviewers a clear map of the change. The "DIDN'T TOUCH" section is especially important; it shows you exercised scope discipline and didn't go on an unsolicited renovation.
| "Branches add overhead" | Short-lived branches are free and prevent conflicting work from colliding. Long-lived branches are the problem; merge within 1-3 days. |