- **Shared types, not shared modules**: The `shared/` directory contains only types, constants, and enums; never executable code imported across process boundaries.
    // VALIDATE INPUTS; never trust renderer data blindly
// src/renderer/App.tsx; or any renderer code
> ⚠️ **Never** use `ipcRenderer.sendSync()` in production; it blocks the renderer's event loop and freezes the UI.
[ ] webContents 'will-navigate' event intercepted; block unexpected URLs
- ✅ Audit `node_modules` shipped with the app; use `electron-builder`'s `files` exclude patterns
- ❌ Do NOT bundle the entire `node_modules`; only production dependencies
- **electron-vite** or **electron-forge with Vite plugin**; modern, fast HMR for renderer
- **tsx** or **ts-node**; for running TypeScript in main process during development
- **concurrently**; run renderer dev server + Electron simultaneously
- ✅ **Always** validate IPC inputs in the main process; treat renderer as untrusted
- ❌ **Never** set `nodeIntegration: true`; this is the #1 Electron security vulnerability
- ❌ **Never** use `ipcRenderer.sendSync()`; it blocks the renderer event loop
- `chrome-extension-developer`; When building browser extensions instead of desktop apps (shares multi-process model concepts)
- `docker-expert`; When containerizing Electron's build pipeline or CI/CD
- `react-patterns` / `react-best-practices`; When using React for the renderer UI
- `typescript-pro`; When setting up advanced TypeScript configurations for multi-target builds
- `nodejs-backend-patterns`; When the main process needs complex backend logic
- `github-actions-templates`; When setting up CI/CD for cross-platform Electron builds