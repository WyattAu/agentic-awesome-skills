A Progressive Web App is a web application that uses modern browser capabilities to deliver a fast, reliable, and installable experience; even on unreliable networks. Common building blocks (install criteria vary by browser) are:
1. **HTTPS**; Required in production for service workers to register (localhost is exempt for development).
2. **Web App Manifest** (`manifest.json`); Makes the app installable and defines its appearance on device home screens.
3. **Service Worker** (`sw.js`); A background script that intercepts network requests, manages caches, and enables offline functionality.
- Use when the user asks "can my website be installed like an app?" or "how do I make my site work offline?"; even if they don't use the word PWA.
- [ ] `index.html`; Links manifest, registers service worker
- [ ] `manifest.json`; Full app metadata and icon set
- [ ] `sw.js`; Service worker with install, activate, and fetch handlers
- [ ] `app.js`; Main app logic with SW registration and install prompt handling
- [ ] `offline.html`; Fallback page shown when navigation fails offline (required by this example's cache.addAll list, not by the PWA standard)
- `purpose: "maskable"` on icons enables adaptive icons on Android (safe zone matters; keep content in center 80%).
// Capture the browser's install prompt; it fires before the browser's own UI
### Cache Versioning (critical: always increment on deploy)
### Install: Pre-cache the App Shell
### Activate: Clean Up Old Caches
### Fetch: Caching Strategies
  // Strategy A: Cache-First (for static assets; fast, tolerates stale)
  // Strategy B: Network-First (for HTML pages; fresh, falls back to cache)
- Safari supports manifests and service workers but **does not support `beforeinstallprompt`**; users must install via the Share → "Add to Home Screen" menu manually.
// With Workbox (via CDN for simplicity; use npm + bundler in production)