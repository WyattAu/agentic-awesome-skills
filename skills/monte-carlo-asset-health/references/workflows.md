Referenced from the main SKILL.md; consult when executing the workflow.
### Phase 1: Resolve the asset
### Phase 2: Gather health metrics (ALL in parallel)
→ monitor configs; check status field for paused vs active
date; e.g. `created_before` = now (`2026-07-10T00:00:00Z`) and `created_after`
### Phase 3: Check upstream health (ALL parents in parallel)
10 and note: "N more upstream parents not checked; ask to see more."
### Phase 4: Synthesize the health report
1. **Tags**; from `search` properties. Show None only after a successful result confirms no tags; otherwise Unknown.
2. **Status**; determine from alerts and monitoring:
4. **Metrics table**; Last Activity (only if returned), alerts, monitoring, upstream; put importance and read/write averages in the header
5. **Active Alerts**; list each with type and status
6. **Upstream Issues**; list each checked parent with observed alerts/metadata and identify unchecked parents; no-alert results do not prove health
7. **Recommendations**; only facts derivable from data:
### Phase 1: Get upstream of the specified parent
### Phase 2: Check grandparent health (ALL in parallel)
### Phase 3: Report