A quota-aware, parallel subagent coordination skill for Antigravity 2.0. Turns one big task into a set of isolated, efficient agent missions; without burning your weekly quota.
## Phase 1: DECOMPOSE (before any agent runs)
## Phase 2: QUOTA ROUTING
## Phase 3: CONTEXT ISOLATION
AGENT CONTEXT PACKET; agent-[ID]
Rule: If an agent doesn't need `node_modules`, `package-lock.json`, `.next/`, or `dist/`; exclude them in the supported context mechanism when appropriate; do not hide the lockfile or files needed for the actual task.
## Phase 4: PARALLEL EXECUTION
## Phase 5: ERROR RECOVERY
## Phase 6: INTEGRATION CHECK
- If blocked, say why explicitly; never just stop
- `nextjs-feature.md`; Building a full Next.js feature with 3 parallel agents
- `api-plus-frontend.md`; Backend API agent + Frontend UI agent running in parallel
- `debug-mission.md`; Repair mission for a broken build using minimal quota