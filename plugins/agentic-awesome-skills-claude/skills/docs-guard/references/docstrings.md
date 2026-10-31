# Docs Guard: Docstring, PHPDoc, and JSDoc Rules
- Public API surface: always; it feeds IDEs, generated references, and agents.
- Internal helpers: only when the contract is not expressible in the signature (units, invariants, side effects, "why"). An internal one-liner with an intention-revealing name usually needs nothing; and clean-code-guard's comment rules apply.
// Fails the test; restates the obvious, documents nothing.
- `@param` names and order match the signature exactly; drift here actively lies to IDEs.
- `@throws` lists what the body actually throws; verify each raise site; remove what no longer throws.
- A wrong docstring becomes a published wrong reference page; Rule 1 severity applies as if it were the README.
- Check that examples inside docstrings obey [code-samples.md](code-samples.md); they are the least-reviewed samples in any codebase.