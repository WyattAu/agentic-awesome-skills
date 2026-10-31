description: "Interactively debug source code; set breakpoints, step through execution line by line, inspect live variable state, evaluate expressions against the running program, and navigate the call stack to trace root causes."
Use this skill when you need interactively debug source code; set breakpoints, step through execution line by line, inspect live variable state, evaluate expressions against the running program, and navigate the call stack to trace root causes. Use when a program crashes, raises unexpected exceptions, produces...
how execution reached a particular state; and running it again with more print statements
evaluate arbitrary expressions against the live process; all without restarting.
- **Have a hypothesis**; set a breakpoint where you expect the bug: `dap debug script.py --break script.py:42`
- **Conditional breakpoint**; only stop when a condition is met: `dap debug script.py --break "script.py:42:x > 5"` (
- **Multi-file app**; breakpoints across modules: `--break src/api/routes.py:55 --break src/models/user.py:30`
- **No hypothesis, small program**; walk from entry: `dap debug script.py --stop-on-entry` (avoid for large projects -
- **Exception, location unknown**; `dap debug script.py --break-on-exception raised` (Python) / `all` (Go/JS)
- **Remote process**; `dap debug --attach host:port --backend <name>`
- **Process already running (stuck server, live issue)**; attach without restarting:
If you expected `compute()` to be called, but it never is, then the bug is in the caller; not `compute()`, but whatever
caller passed. Keep going up (`--frame 2`, `--frame 3`) until you find the frame where the value first became wrong -
Before setting a breakpoint: *"I believe the bug is in X because Y."* A good hypothesis is falsifiable; your next
- Exception at line 80? Root cause is upstream; start earlier
- Uncertain? Bisect: `--break f:20 --break f:60`; wrong state before or after halves the search space
- **Boundaries**; where data crosses a format, representation, or module boundary; state is cleanest here
- **State transitions**; the line that assigns or mutates the corrupted value
- **Wrong branch**; the condition whose inputs led to the bad path
- **Antipatterns**; don't break inside library code; break at the call site instead. Don't use unconditional breaks in
  tight loops; use conditions.
served their purpose; progressive narrowing without restarting:
Stop only when a condition is true; essential for loops, hot paths, and specific input values.
Conditional breakpoints as runtime assertions; stop the *moment* something goes wrong:
dap step                         # step over; trust this call, advance to next line
dap step in                      # step into; suspect what's inside this function
dap step out                     # step out; you're in the wrong place, return to caller
Avoid eval expressions that call methods with side effects; they mutate program state and can corrupt your debugging
`dap continue --to file:line`. It's a disposable breakpoint; stops once, then vanishes. Good for
For advanced scenarios; hangs, concurrency bugs, deeply nested state, loop bisection -
breakpoints; a fast feedback loop. Don't trust that a fix works until you've observed the correct