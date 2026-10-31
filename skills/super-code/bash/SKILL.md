# ✅: $() nests cleanly
# ✅: [[ is safer, supports &&/||, no word splitting inside
# ✅: test command directly
# ✅: (( )) for arithmetic
# ✅: glob directly
# ✅: brace expansion (Bash)
    (( ++count ))  # count resets after loop; subshell
# ✅: redirect, not pipe
# ✅: single grep with pattern
# ❌ cat + pipe (UUOC: Useless Use of Cat)
# ✅: process substitution
echo $?  # 0; false's failure hidden
# ✅: echo + capture
# ✅: use local, return via stdout
# ❌ No error handling: script continues after failure
# ✅: trap for cleanup
# ✅: redirect only when you know what you're suppressing