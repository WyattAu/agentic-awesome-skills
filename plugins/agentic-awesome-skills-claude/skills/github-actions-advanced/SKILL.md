# Workflow-level default: restrict everything
    # Job-level override: only expand what's needed
# ❌ UNSAFE: tag can be mutated or hijacked
# ❌ UNSAFE: attacker controls PR title, which gets expanded in shell
# ✅ SAFE: pass through environment variable (shell doesn't evaluate it)
# ✅ SAFE: expressions in if: conditions are evaluated by Actions, not shell