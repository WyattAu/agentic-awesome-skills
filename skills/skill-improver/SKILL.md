Verify it's enabled: run `/plugins`; `plugin-dev` should appear in the list. If missing, report the prerequisite and verify the provider and installation instructions before an authorized install; the plugin name alone does not establish its source.
- Missing required frontmatter fields (name, description); Claude cannot index or trigger the skill
- Invalid YAML frontmatter syntax; Parsing fails, skill won't load
- Referenced files that don't exist; Runtime errors when Claude follows links
- Broken file paths; Same as above, leads to tool failures
- Weak or vague trigger descriptions; Claude may not recognize when to use the skill
- Writing voice that materially obscures execution; assess against the target project style, not a universal second-person prohibition
- Excessive context use; inspect actual loading behavior and consider references; 500 lines is a guideline unless the host/project enforces it
- Description doesn't specify when to trigger; Skill may never be selected
- Subjective style preferences; Reviewer may have different taste than author
- Optional enhancements; May add complexity without proportional value
- "Nice to have" improvements; Consider cost-benefit before implementing
- Formatting suggestions; Often valid but low impact
**Iteration 1; skill-reviewer output:**
**Iteration 2; run skill-reviewer again to verify fixes:**