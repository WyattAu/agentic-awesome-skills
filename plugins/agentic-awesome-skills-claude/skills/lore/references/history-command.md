# `lore history`: full specification
1. [DEC-2026-07-10-ee31] (scopes/backend/DECISIONS.md); SHA-256 + salt
2. [DEC-2026-07-10-e45d] (scopes/backend/DECISIONS.md); bcrypt (rounds=12)
- `0`; success (including "0 commits found" case)
- `2`; usage / configuration error
- `3`; entry lookup failure
- `4`; not a git repo
- `5`; git CLI missing
- `6`; invalid scope
- `7`; git command failed