# Architecture Audit Guide: Mode 2
1. **Top-level structure**; glob top two levels to identify module boundaries:
2. **Entry points**; read the package manifest or main config file (e.g., `package.json`,
3. **Dependency edges**; grep import statements to discover inter-module calls. Run once
4. **Large modules**; for any top-level directory with > 10 files, read the file matching
Draw the graph structure first; nodes, subgraphs, and edges; without any `classDef` or
1. **Nodes**; Use top-level directories or services as nodes, not individual files
2. **Grouping**; One `subgraph` per architectural layer or top-level directory (e.g., UI, Domain, Infrastructure)
3. **Edges**; Solid arrows (`-->`) point FROM the depending module TO the dependency; use dotted arrows with label (`-.->|circular|`) for circular dependencies. If no circular dependencies exist, use only solid arrows
4. **Node limit**; Keep the graph to ~50 nodes maximum; collapse low-risk leaf modules into their parent if needed
5. **Fan-out**; For any node with fan-out > 5, use a descriptive label: `HighFanOutModule["ModuleName (fan-out: 7)"]`
6. **Colors**; Apply `classDef` colors AFTER completing Steps 2-4: `critical` (red `#ff6b6b`) for nodes with Critical findings, `warning` (yellow `#ffd43b`) for Warning findings, `clean` (green `#51cf66`) for nodes with no findings or only Suggestions. If no findings were observed in a reviewed node, label it no findings observed; sampled/unread nodes remain unknown, not clean
7. **Direction**; Default to `graph TD` (top-down); use `graph LR` only if the architecture is clearly a left-to-right pipeline
*The most architecturally consequential risk; scan this first.*
code; typically an interface, a configuration point, or a dependency injection boundary.
  boundary; any change requires touching the entire call stack to substitute behavior.
Source: Feathers; Working Effectively with Legacy Code, Ch. 4: The Seam Model
- 🔴 Critical: the Payments module is owned by Team A but contains auth logic owned by Team B -
  things; theoretically painful but not yet causing release coordination issues
- Not a finding: a single team owns a monorepo with multiple logical modules; Conway's Law