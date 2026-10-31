Generate production-quality React + Tailwind CSS code using the Rayden UI component library (34 components). The skill loads a complete API reference with every component, every prop, design tokens, layout patterns, and an explicit anti-pattern ban list; preventing hallucinated components and generic AI output. Built on the Rayna UI design system.
1. **Parses the request**; Identifies page type, required components, and data model
2. **Loads RAYDEN_RULES.md**; Complete reference: 34 components with full props, design philosophy, token classes, layout patterns, anti-patterns, and accessibility rules
3. **Plans the layout**; Decides page structure, component selection, spacing, color, and elevation strategy
4. **Generates code**; Writes React + Tailwind CSS using only documented components and token classes
5. **Self-validates**; Runs a 16-point checklist covering correctness (valid components/props, token usage, nesting) and design quality (whitespace, hierarchy, restraint, responsiveness)
**Use case:** You're building an internal analytics tool and need a full dashboard page with MetricsCard grid, sortable Table, and ActivityFeed sidebar; all with correct Rayden imports and token classes.
**Use case:** You need a centered auth form with Input components, a primary Button, and proper visual hierarchy; following Rayden's "Auth / Focused Form" pattern.
**Use case:** You're building a storefront and need a responsive product grid with Chip filters, Input search, Pagination, and Cards with images; all using Rayden's layout and spacing rules.
- Describe what you want in plain language; the skill maps your request to the right components
- Review generated code for business logic; the skill handles UI, not data fetching
| "Component doesn't exist" error | The skill only uses documented components; check if you're asking for something Rayden doesn't have |
| Layout not responsive | The skill generates responsive code by default; check that your viewport meta tag is set |
- `rayden-use`; Build Rayden UI components and screens in Figma via MCP (included in the same package)