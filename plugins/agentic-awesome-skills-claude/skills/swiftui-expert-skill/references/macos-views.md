> - **`NavigationSplitView`**; sidebar-based navigation (sidebar drives content/detail)
> - **`HSplitView`/`VSplitView`**; IDE-style layouts where all panes are equal peers
> **Warning:** Never set `frame`/`bounds` directly on the managed `NSView`; SwiftUI owns the layout.
Host SwiftUI content inside AppKit (reverse direction; AppKit app embedding SwiftUI views).
- **Use `NavigationSplitView`** for sidebar-driven navigation; reserve `HSplitView`/`VSplitView` for IDE-style equal peer panes
- **Make `Table` adaptive**; handle compact size classes by showing combined info in the first column
- **Always call `startAccessingSecurityScopedResource()`** on URLs from `fileImporter`; they are security-scoped
- **Use `Transferable`** for drag & drop (modern); fall back to `NSItemProvider` only for legacy compatibility
- **Never set `frame`/`bounds`** directly on views managed by `NSViewRepresentable`; SwiftUI owns the layout
- **Prefer native SwiftUI** over AppKit interop when possible; only use `NSViewRepresentable` for features SwiftUI doesn't provide