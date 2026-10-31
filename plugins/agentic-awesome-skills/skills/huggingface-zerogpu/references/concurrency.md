# BAD: concurrent requests overwrite each other
# GOOD: pure function, no shared mutable state
Note that on ZeroGPU, `gr.State` is pickled across the worker boundary on every yield; see "Process Isolation and Pickle" in SKILL.md for the implications.
# BAD: concurrent calls clobber the same file
# GOOD: unique path per invocation
# SAFE: loaded once at module scope, read-only during requests