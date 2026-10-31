// Fork child to ptrace parent; blocks all other debuggers
// Looks for "TracerPid:\t0"; non-zero means debugger
// /proc/self/maps; check for debugger libraries
// SIGTRAP handler; INT3 under debugger is caught by debugger, not handler
// SIGALRM timeout; kill self if analysis takes too long
// Direct syscall instead of libc; bypasses LD_PRELOAD hooks
// ProcessDebugFlags (0x1F); returns inverse: 0 = debugger present
// UnhandledExceptionFilter; under debugger, filter is NOT called
// INT 2D; debugger single-step anomaly
// Hide thread from debugger; stops all debug events
// Instruction count timing; DBI adds overhead
4. **Emulate** instead of debug (Unicorn/Qiling; no code modification)
mov eax, 1        ; Real code; disassembler may misalign here
e8             ; fake CALL opcode; disassembler tries to decode as call
- **d810-ng** (223⭐, IDA, 集成 Z3, 覆盖 OLLVM/Tigress/Hodur/Approov): https://github.com/w00tzenheimer/d810-ng; **本地首选**
Binary uses SIGFPE signal handlers for control flow, making static analysis unreliable. Brute-force by counting SIGFPE signals via strace; correct input characters produce more signals.
UPX-packed binary applies a sequence of arithmetic-only transforms (sub, add, xor, rol, ror) to the flag. No memory side-effects; purely register arithmetic. IDAPython traces non-jump instructions, the sequence is then inverted to recover the flag.
**Key insight:** Arithmetic-only obfuscation (no memory writes) is fully reversible by tracing, inverting the instruction sequence, and swapping inverse operations. PEB anti-debug can silently change comparison targets; always verify which branch is taken.
1. **Identify all anti-analysis checks**; search for: `ptrace`, `IsDebuggerPresent`, `rdtsc`, `cpuid`, `NtQuery`, `GetTickCount`, `CheckRemoteDebuggerPresent`, `/proc/self`, `SIGTRAP`, `alarm`
2. **Static patching**; NOP/patch checks with pwntools or Ghidra before running
3. **LD_PRELOAD** (Linux); hook libc functions returning fake values
4. **ScyllaHide** (Windows x64dbg); patches PEB, hooks NT functions automatically
5. **Emulation** (Unicorn/Qiling); no debugger artifacts to detect
6. **Kernel-level bypass**; modify `/proc/sys/kernel/yama/ptrace_scope`, use documented per-process settings only within an authorized test VM