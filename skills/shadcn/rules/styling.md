For positive, negative, or status indicators, use Badge variants, semantic tokens like `text-destructive`, or define custom CSS variables; don't reach for raw Tailwind colors.
1. **Built-in variants**; `variant="outline"`, `variant="destructive"`, etc.
2. **Semantic color tokens**; `bg-primary`, `text-muted-foreground`.
3. **CSS variables**; define custom colors in the global CSS file (see [customization.md](../customization.md)).
Use semantic tokens; they handle light/dark via CSS variables. `bg-background text-foreground` not `bg-white dark:bg-gray-950`.