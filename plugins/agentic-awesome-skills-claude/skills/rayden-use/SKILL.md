Build and maintain Rayden UI components and screens directly in Figma using the Figma MCP. The skill enforces the Rayna UI design system; resolved design tokens, craft rules, anti-pattern detection, and visual validation; so every output is mechanically correct and visually premium. Supports three style modes (conservative, balanced, expressive) and includes a dedicated subagent for full-page screen composition.
1. **Verifies environment**; Checks Figma MCP connection and write access via `whoami`
2. **Loads component data**; Reads Rayden component specs, anatomy, and tokens from the `@raydenui/ai` MCP server or installed package
3. **Loads craft rules**; Reads supporting files: resolved token values, craft rules, anti-patterns, and screen layout patterns
4. **Identifies task type**; Determines if building a single component, composing a screen, auditing, or adding variants
5. **Applies style mode**; Adjusts spacing, shadow, typography, and visual weight based on conservative/balanced/expressive mode
6. **Builds with helpers**; Generates Figma Plugin API code using mandatory helper functions (hexToRgb, loadFonts, applyShadow, applyBorder) with auto layout on every frame
7. **Visual validation**; Takes screenshots after each build stage and validates against 8 acceptance criteria (alignment, spacing, color accuracy, hierarchy, radius, shadow, primary action count)
**Use case:** You're designing an analytics dashboard and need a sidebar layout with KPI cards, a data table, and an activity feed; all using consistent Rayden tokens and spacing.
**Use case:** The Input component exists in your Figma file but is missing error and success states; the skill reads the existing structure and extends it.
- Let the skill take screenshots between build stages; this is how it validates output quality
- Review the generated output in Figma after completion; the skill validates mechanically but human judgment on aesthetics is still valuable
| "Font not found" error | The skill falls back to Roboto if Inter is unavailable; ensure Inter is loaded in your Figma file for best results |
- `rayden-code`; Generate React code with Rayden UI components (included in the same package)
- `rayden-compose`; Dedicated subagent for composing full-page Figma screens (included in this skill package)