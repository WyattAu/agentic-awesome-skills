- **Web-only libraries**; Charts (recharts, chart.js), syntax highlighters, rich text editors, or any library that depends on DOM APIs
- **Migrating web code**; Bring existing React web components to native without rewriting
- **Complex HTML/CSS layouts**; When CSS features aren't available in React Native
- **iframes or embeds**; Embedding external content that requires a browser context
- **Canvas or WebGL**; Web graphics APIs not available natively
- **Native performance is critical**; Webviews add overhead
- **Simple UI**; React Native components are more efficient for basic layouts
- **Deep native integration**; Use local modules instead for native APIs
- **Layout routes**; `_layout` files cannot be DOM components
2. **Single default export**; One React component per file
3. **Own file**; Cannot be defined inline or combined with native components
4. **Serializable data props**; Strings, numbers, booleans, arrays, plain objects; supported async native-action callbacks are the explicit exception below
5. **Include CSS in the component file**; DOM components run in isolated context
- Keep DOM components focused; don't put entire screens in webviews
- Test on all platforms; web rendering may differ slightly from native webviews
- Large DOM components may impact performance; profile if needed
- The webview has its own JavaScript context; cannot directly share state with native