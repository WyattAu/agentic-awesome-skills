# ✅: pattern match directly
# ✅: with clause
# ✅: multi-clause function with pattern match
# ✅: guard clause
# ✅: use then/1 or named function
# ✅: direct call for single operation
# ✅: GenServer for stateful processes
# ✅: Task for fire-and-forget with supervision
# ✅: use Registry or named GenServer
# ✅: let it crash; supervisor restarts
# ✅: tagged tuples for expected outcomes
# ✅: handle both cases
# ✅: atom or struct errors (matchable, cheap)
# ✅: for comprehension when filter + transform
# ❌ Enum on large dataset (eager: builds intermediate lists)
# ✅: Stream for lazy evaluation
# ✅: struct enforces keys
# ✅: just a function until you need polymorphism