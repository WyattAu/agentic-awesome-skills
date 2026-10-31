| **Tier 1; Read-Only** | `Read`, `Grep`, `Glob` | Low | No direct write tools; reads can still expose private data and untrusted instructions |
| **Tier 2; Execution** | `Bash` | Medium | Can run arbitrary commands; should have clear justification |
| **Tier 3; Modification** | `Write`, `Edit`, `NotebookEdit` | High | Can modify files; verify the skill needs to create/edit files |
| **Tier 4; Network** | `WebFetch`, `WebSearch` | High | Can access external URLs; verify domains are necessary |
| **Tier 5; Delegation** | `Task` | High | Can spawn subagents; increases attack surface |
| **Tier 6; Unrestricted** | `*` (wildcard) | Critical | Full access to all tools; almost never justified |
| `Read` | Skill reads files for analysis |; (almost always justified) |
| `Grep` | Skill searches file contents |; (almost always justified) |
| `Glob` | Skill finds files by pattern |; (almost always justified) |
- **If seen**: Requires strong justification; the skill should be doing something that genuinely needs broad access
| `Task` + `Bash` + `Write` | Can spawn subagents with write access; high risk |