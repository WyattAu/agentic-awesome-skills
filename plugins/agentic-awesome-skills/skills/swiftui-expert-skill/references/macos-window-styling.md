// Unified compact; minimal chrome
// Expanded; title bar above toolbar
// Hidden title bar; chromeless window
- **Use `.unified` or `.unifiedCompact`** for most apps; `.expanded` only when you need many toolbar items
- **Use `NavigationSplitView`** for sidebar navigation; not `HSplitView`
- **Use `Inspector`** for supplementary panels; it integrates with the toolbar automatically
- **Define `Commands`** for all repeatable actions; users expect keyboard shortcuts on macOS