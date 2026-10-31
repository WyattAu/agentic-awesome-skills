also let agents quickly locate specific information; "jump to section 3.2" is unambiguous.
    subgraph "Layer 3; Entry Points"
    subgraph "Layer 2; Business Logic"
    subgraph "Layer 1; Utilities"
    subgraph "Layer 0; Types"
| L3 | \`cmd/\` | L0, L1, L2 |; |
These are not style suggestions; they are mechanically enforced by the linter:
├── cmd/          ; CLI commands (Layer 3)
│   ├── core/     ; Business logic (Layer 2)
│   ├── types/    ; Type definitions (Layer 0)
│   └── utils/    ; Utilities (Layer 1)
├── docs/         ; Documentation
├── scripts/      ; Linters and tooling
├── harness/      ; Eval & quality
└── tests/        ; Test fixtures
| \`API_KEY\` |; | Yes | API authentication |
// ✓ Good; structured, parseable, queryable
// ✗ Bad; unstructured, hard to parse
// ✓ Good; typed, machine-readable
// ✗ Bad; stringly typed
// ✓ Good; preserves error chain
// ✗ Bad; loses context
- [Architecture Overview](../ARCHITECTURE.md#31-component-name); Section 3.1
- [Related Component](related.md); How it interacts with this component
| \`NewClient(cfg)\` |; | Create API client |