- Chrome DevTools MCP Server; enables visual verification of generated pages
- Section 4 (Sitemap); Do NOT recreate pages that already exist
- Section 5 (Roadmap); Pick tasks from here if backlog exists
- Section 6 (Creative Freedom); Ideas for new pages if roadmap is empty
     - `htmlCode.downloadUrl`; Download and save as `.stitch/designs/{page}.html`
      - `screenshot.downloadUrl`; Append `=w{width}` to the URL before downloading, where `{width}` is the `width` value from the screen metadata (Google CDN serves low-res thumbnails by default). Save as `.stitch/designs/{page}.png`
│   ├── next-prompt.md  # The baton; current task
The skill is orchestration-agnostic; focus on the pattern, not the trigger mechanism.