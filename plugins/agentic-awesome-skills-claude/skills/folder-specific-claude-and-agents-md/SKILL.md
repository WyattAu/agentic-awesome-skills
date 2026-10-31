**Only create a file if the folder has context needed across multiple sessions**; active evolving work, specific conventions, ongoing decisions. A folder of static reference files does NOT need one (agents can read on demand). If unsure, ask the user.
Before writing the file, give the user a bullet list grouped by section; let them react first. Candidate sections (skip any that don't apply):
- **Product / Purpose**; what this folder/project is, current state, key metrics
- **Avatar / Audience**; who it's for (if applicable)
- **Essential Files**; one-line role for each important file, including cross-folder references (use `@path/file.md` import syntax)
- **Constraints (MUST NOT)**; explicit hard negatives. Highest-ROI content in the file.
- **Conventions**; the user's lingo, status emojis (✅ 🟡), naming patterns, "usually do" patterns
- **Locked Decisions**; things agreed + dated, must not re-litigate
- **Context**; history, authority, credibility that frames the work
- **How to work with the user**; collaboration style for this specific folder
- **Marketing Angles / Positioning**; if public-facing
- **Top Insights**; 3-5 most glaring signals from research (if research exists)
- **No file trees, no directory dumps, no stack details the code already shows.** Anything an agent can derive from `ls` or `grep` rots fast and wastes tokens. Pin decisions, rules, and context; not structure.
- **No emojis unless the user uses them** (status markers ✅ 🟡 are the exception; they're already conventions).