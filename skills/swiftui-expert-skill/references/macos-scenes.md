> SwiftUI scene types for macOS apps; `Settings`, `MenuBarExtra`, `WindowGroup`, `Window`, `UtilityWindow`, and `DocumentGroup`. Covers macOS-only scenes and cross-platform scenes with macOS-specific behavior.
- **`.menu`** (default); standard dropdown menu
- **`.window`**; popover panel with custom SwiftUI views
- **Toggleable**; pass `isInserted:` with an `@AppStorage` binding to let users show/hide the extra: `MenuBarExtra("Status", systemImage: "chart.bar", isInserted: $showMenuBarExtra)`
- **Menu-bar-only app**; use `MenuBarExtra` as the sole scene + set `LSUIElement = true` in Info.plist to hide the Dock icon. The app auto-terminates if the user removes the extra from the menu bar.
- **Multiple window instances**; users can open many windows from File > New Window
- **Tabbed interface**; users can merge windows into tabs
- **Automatic Window menu**; commands for window management appear automatically
// Open programmatically; brings to front if already open
For multiple document types, add additional `DocumentGroup` scenes; use `DocumentGroup(viewing:)` for read-only formats.
- **Use `Settings`** for preferences; prefer this over a custom preferences window
- **Use `MenuBarExtra`** for menu bar items; prefer this over managing AppKit's `NSStatusItem` directly
- **Use `WindowGroup`** as the primary scene; reserve `Window` for supplementary singletons
- **Use `UtilityWindow`** for inspectors/palettes; it handles floating, focus, and visibility automatically
- **Use `DocumentGroup`** for document-based apps; it provides the full File menu and document lifecycle
- **Use `openWindow(id:)`** to open windows programmatically; it brings existing windows to front