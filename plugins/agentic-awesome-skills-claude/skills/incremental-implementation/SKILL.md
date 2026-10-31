Build in thin vertical slices; implement one piece, test it, verify it, then expand. Avoid implementing an entire feature in one pass. Each increment should leave the system in a working, testable state. This is the execution discipline that makes large features manageable.
2. **Test**; run the test suite (or write a test if none exists)
3. **Verify**; confirm the slice works as expected (tests pass, build succeeds, manual check)
5. **Move to the next slice**; carry forward, don't restart
If you notice something worth improving outside your task scope, note it; don't fix it:
**Good:** Three separate commits; one for each change.
- Avoid deleting something in one commit and replacing it in the same commit; separate them
Don't touch the UI yet; we'll do that in the next increment.
**Note:** Run each verification command after a change that could affect it. After a successful run, don't repeat the same command unless the code has changed since; re-running on unchanged code adds no information.