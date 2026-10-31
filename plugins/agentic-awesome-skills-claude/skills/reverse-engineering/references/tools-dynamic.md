// hook.js; intercept a function and log arguments/return value
// Bypass ptrace(PTRACE_TRACEME); returns 0 (success) without calling
// Bypass timing checks; hook clock_gettime to return constant
// Trace all calls in a function (Stalker; instruction-level tracing)
// hook_android.js; hook Java method
**Key insight:** Frida excels where static analysis fails; obfuscated code, packed binaries, and runtime-generated data. Hook comparison functions (`strcmp`, `memcmp`, custom validators) to extract expected values without reversing the algorithm. Use `Interceptor.attach` for observation, `Interceptor.replace` for modification.
// memo_hook.js; memoize a recursive function to skip redundant calls
# Hook ptrace syscall: return 0 (success)
**Key insight:** Movfuscated binaries (compiled with `movfuscator`) expand every instruction into sequences of `mov` operations, making static analysis impractical. However, character-by-character comparison still creates measurable instruction count differences. Pin's `inscount0.so` counts total executed instructions; the correct character at each position causes ~1000+ more instructions (proceeding further in the comparison). Also works for obfuscated binaries with sequential input checks.
// Modified inscount0.cpp; count only executions of a specific address
// freeze_time.c; compile: gcc -shared -fPIC -o freeze.so freeze_time.c
# observe output: correct byte produces expected output character.