# re-create: Controlled Erasure & Rebuild Protocol
> Hollow Purple is Gojo's most destructive technique; blue and red combined into total erasure of the target. But Gojo doesn't use it carelessly. He knows exactly what he's erasing and why. Same here: this skill is the nuclear option, invoked only when patching is the wrong call, executed with full control over what gets erased and what must survive.
### PHASE 1: Justify the Erasure
   - Not "it's messy"; specific structural problems that make targeted fixes impossible or counterproductive
If the AI cannot clearly answer all three, it must fall back to targeted edits instead of a rewrite. A rewrite is not a reward for messy code; it is a last resort.
### PHASE 2: Read the Target Completely
1. **Public interfaces**; functions, classes, types, or exports that other parts of the codebase call
2. **Implicit contracts**; behaviors that other files depend on even if not formally typed
3. **Working behaviors**; things the current implementation does correctly that must continue to work
4. **Non-obvious logic**; edge cases, guards, or special handling that looks incidental but is intentional
5. **Blast radius**; every file in the codebase that imports from or depends on the target
> **The AI cannot skip this phase even if it has read the file before.** The purpose is not familiarity; it is building the Preservation List.
### PHASE 3: Erasure Declaration (User Must Confirm)
HOLLOW PURPLE; ERASURE PLAN
  [specific justification; architectural rot, fundamental mismatch, etc.]
  [Description of what the rebuild will look like; structure, approach, key decisions]
  [Anything intentionally dropped and why; dead code, deprecated behavior, etc.]
### PHASE 4: Controlled Erasure
- **Delete cleanly.** Not commented out, not renamed to `_old`, not archived in place; deleted.
### PHASE 5: Rebuild Against the Preservation List
2. **Match the blast radius expectations.** Files that depended on the old implementation must be able to use the new one without changes; unless changes to dependent files were declared in Phase 3.
4. **Follow existing codebase conventions.** The new implementation must use the same patterns, naming conventions, and style as the surrounding codebase; not whatever the AI prefers.
### PHASE 6: Blast Radius Verification
2. **Verify each dependency**; the function signatures, exports, and behaviors it relied on are present in the rebuild
3. **Flag any breakage**; if a dependent file now has a mismatch, report it and propose a fix before declaring done
HOLLOW PURPLE; VERIFICATION
| 1 | Have I read the entire target and built a complete Preservation List? | Yes; or read more |
| 2 | Have I identified the full blast radius? | Yes; or search more |
| 3 | Has the user confirmed the erasure plan? | Yes; or wait |
| 4 | Is the erasure scoped exactly to what was declared? | Yes; or re-declare |
| 1; Justify | Prove erasure is necessary | ❌ No |
| 2; Read | Read target fully, build Preservation List | ❌ No |
| 3; Declare | Output erasure plan, wait for confirmation | ❌ No |
| 4; Erase | Delete the declared target | 🗑️ Deletes only |
| 5; Rebuild | Write new implementation against Preservation List | ✅ Writes |
| 6; Verify | Check Preservation List + blast radius | ❌ No |
AI: [Outputs HOLLOW PURPLE; ERASURE PLAN]