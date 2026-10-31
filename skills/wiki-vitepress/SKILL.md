Use `setup()` with `onMounted`, NOT `enhanceApp()`; DOM doesn't exist during SSR.
- Mermaid renders async; SVGs don't exist when `onMounted` fires. Must poll.
- `isCustomElement` compiler option for bare `<T>` causes worse crashes; do NOT use it
- Node text in Mermaid uses inline `style` with highest specificity; CSS alone won't fix it
- `enhanceApp()` runs during SSR where `document` doesn't exist; use `setup()` only