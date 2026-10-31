# the-honoured-one: Full Context Load Protocol
> Gojo at full power means all six eyes open; everything visible, nothing assumed, no blind spots. The Honoured One doesn't act on guesses. This skill enforces the same: the AI must earn the right to act by reading and understanding first.
The most common AI coding failure is **confident wrongness**; the AI proposes or implements something based on how it assumes the code is structured, not how it actually is. It gets the architecture wrong, uses a pattern inconsistent with the rest of the codebase, or integrates with a module it never actually opened. This skill reduces that failure mode by making relevant context-loading explicit; reading alone cannot guarantee correctness.
### PHASE 1: Context Audit
1. **What files are relevant to this task?**; Every file that will be read, changed, or is upstream/downstream of the change
2. **Which of those has the AI actually read this session?**; Honest accounting, no assumptions
3. **What gaps exist?**; Files that are relevant but unread
THE HONOURED ONE; CONTEXT AUDIT
Cannot proceed; reading blind spots now.
### PHASE 2: Mandatory Read Pass
The AI reads every file listed as a blind spot. Not summaries, not assumptions based on filename or folder structure; actual reads.
- The AI does not form opinions or solutions while reading; this phase is observation only
### PHASE 3: Orientation Statement
THE HONOURED ONE; CONTEXT LOADED
   based on what was read; not assumed]
  - [anything still unclear; or "None, ready to proceed"]
### PHASE 4: Confidence Gate
| 1 | Have I read every file this task touches? | Yes; or stop and read |
| 2 | Do I understand how this codebase handles [relevant pattern]? | Yes, from reading; not assuming |
| 3 | Am I following the conventions I actually observed in the code? | Yes; or flag the deviation |
| 4 | Do I have any remaining blind spots? | No; or resolve them first |
- **No proposing solutions before reading.** Proposals based on assumptions are not proposals; they are guesses.
| 1; Audit | List relevant files, identify blind spots | ❌ No |
| 2; Read | Read all blind spot files | ❌ No |
| 3; Orient | Output orientation statement | ❌ No |
| 4; Gate | Confirm no unknowns remain | ✅ Yes, if gate passes |