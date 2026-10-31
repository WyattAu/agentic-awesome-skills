   - **Readability**: do names say what things are; does each function do one thing; are magic numbers named. Flag only what you can't understand; not "I'd write it differently."
3. **Write the comments**: fixed format; `[axis] file:line problem → suggested fix`. Only actionable suggestions; "could be optimized" is not one.
- No style policing: indentation, quotes, semicolons; that's the linter's job, not a human's.
- ❌ Drive-by LGTM: approving before reading the whole diff; the review is theater.
- ❌ Comments without code: "this logic looks off"; which logic? Which line? Unsaid means invalid.
- ❌ Performance speculation: "this loop might be slow at scale"; a performance comment without data is noise.