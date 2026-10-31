**CREATE**; Add a new task
**LIST**; Browse tasks with summary stats
**SEARCH**; Find tasks by criteria
**GET**; Full task details (including dependency info and related task details)
**MARK**; Update status with required response message
**UPDATE**; Modify task fields
**ARCHIVE**; Soft delete (preserves data, sets status to archive)
**DEPS**; View, add, or remove task dependencies
**READY**; Tasks with all blockers satisfied (safe to work on)
**ORDER**; Topological sort of open tasks respecting dependencies
[blocked_by]TASK_ID[/blocked_by]         ; This task is blocked by TASK_ID
[blocked_by]ID1, ID2[/blocked_by]        ; Blocked by multiple tasks
[task_id]RELATED_ID[/task_id]            ; Reference a related task
[task_id]ID1 ID2[/task_id]               ; Multiple related tasks
3. **Always include `--response`** when using `mark`; document what you did and how you tested it
10. **Concurrent features are supported**; start each selected task with `yy task start TASK_ID`; each gets a dedicated product worktree, while `yy merge` serializes only target updates
YYLO Ledger mutation resolves the controller in this order: explicit `JUNO_TASK_ROOT`, repository-local registration, then the current project root. Diagnose before orchestration with `.juno_task/scripts/controller_resolver.py --cwd "$PWD" --operation kanban`. The resolver may bootstrap or idempotently confirm a registration, but changing an existing controller requires `yy migrate registration plan` followed by a separately authorized apply. Explicit/registered path or branch errors fail closed-YYLO Ledger never switches Git branches or falls back silently.
- `JUNO_TASK_ROOT`; Explicit canonical controller/task-storage root (not the product `TASK_ROOT`)
- `JUNO_CONTROLLER_BRANCH`; Expected controller branch for environment-based routing
- `JUNO_WORKSPACE_ROLE`; `controller`, `task`, or `integration-owner`
- `JUNO_WORKSPACE_ENFORCEMENT`; `off`, `warn`, or `strict`
- `JUNO_DEBUG=true`; Show diagnostic messages
- `JUNO_VERBOSE=true`; Show informational messages
- `JUNO_KANBAN_LIST_BODY_TRUNCATE_CHARS=N`; Override list body truncation (default: 1200)