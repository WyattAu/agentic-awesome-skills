The single-task loop scales to a queue, and that's where delegation pays off most; a removal split
For anything longer than two or three tasks; especially a run the human steps away from; maintain a
- **Status table**; each task: queued / at-implementer / reviewed+committed (with the commit hash).
- **Per-task review notes**; what landed, what you verified, the gate outcome. One short paragraph.
- **"Needs your eyes"**; design decisions Grok made, non-blocking nitpicks, anything you want the
- **End-of-run checklist**; what happens after the last task (push, open/update the PR, manual checks
Update it as each task lands, not in a batch at the end; if the run is interrupted, the file is still
- Run the full test/build once more on the final tree; not just the last task's slice.
Proceed without asking on anything that follows from the agreed plan; that's the point of the human
Then report where you are, what's committed, and what the open question is; and wait. A queue that