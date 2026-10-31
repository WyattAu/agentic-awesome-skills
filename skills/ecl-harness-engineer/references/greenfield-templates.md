Complete, ready-to-use templates for 0→1 project scaffolding. Each section provides real file contents; not placeholders; for bootstrapping a new project with core harness infrastructure.
- [Shared Templates](#shared-templates) (docs, exec-plans, quality; same for all stacks)
// Layer 0: Core types; no internal imports allowed
 * Layer 0: src/types/    ; No internal imports
 * Layer 1: src/core/     ; May import types only
 * Layer 2: src/commands/ ; May import core and types
 * Layer 3: src/index.ts  ; May import anything
            `✗ ${filePath}:${i + 1}; Layer ${rule.layer} cannot import "${importPath}"`,
          `✗ ${filePath}:${i + 1}; Use structured logger instead of console.log`,
Layer 0: src/{package}/types.py    ; No internal imports
Layer 1: src/{package}/core/       ; May import types only
Layer 2: src/{package}/api/        ; May import core and types
Layer 3: src/{package}/main.py     ; May import anything
                    f"  ✗ {filepath}:{i}; Use logging instead of print()\n"
- [Architecture Overview](docs/ARCHITECTURE.md); System design, layers, data flow
- [Development Setup](docs/DEVELOPMENT.md); Build, test, environment
- [Active Plans](docs/exec-plans/active/); Work in progress
- [Completed Plans](docs/exec-plans/completed/); Historical record
| \`docs/\` |; | All documentation |
| \`scripts/\` |; | Linters and tooling |
| \`harness/\` |; | Eval and quality |
> Status: Greenfield; structure defined, implementation pending
    subgraph "Layer 2; Entry Points"
    subgraph "Layer 1; Business Logic"
    subgraph "Layer 0; Types"
| L2 | \`cmd/\` | L0, L1 |; | CLI commands, entry points |
- [Development Setup](DEVELOPMENT.md); Build and test commands
- [Quality Score](QUALITY_SCORE.md); Quality tracking
| Core | N/A | 0 |; | New project |
├── unit/         ; Fast, isolated tests
├── integration/  ; Tests with real dependencies
└── fixtures/     ; Shared test data
├── cmd/          ; CLI commands (Layer 2)
├── internal/     ; Private packages
│   ├── types/    ; Core types (Layer 0)
│   └── core/     ; Business logic (Layer 1)
├── docs/         ; Documentation
├── scripts/      ; Linters and tools
└── harness/      ; Eval and quality
Format: `{dependency-name}-llms.txt` style; concise, focused on API surface and common patterns.