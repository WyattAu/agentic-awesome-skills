# ✅: generator expression works directly in sum()
# ✅: same thing, just be explicit about defaultdict; it IS the right tool
# (this example is already correct: don't replace defaultdict with a loop)
# ✅: use keyword args at call site for boolean/ambiguous params
# ✅: split only if each part is reused OR independently testable
# ✅: module-level functions; classes for state + behavior
# (see above: use @dataclass)
# ✅: catch the specific exception; don't swallow silently
**Don't add type hints to every local variable; annotate function signatures and class fields; leave obvious locals inferred.**