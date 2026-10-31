1. **Zero Dependencies**; Single HTML files with inline CSS/JS. No npm, no build tools.
2. **Show, Don't Tell**; Generate visual previews, not abstract choices. People discover what they want by seeing it.
3. **Distinctive Design**; No generic "AI slop." Every presentation must feel custom-crafted.
4. **Progressive Disclosure**; Read lightweight style indexes first. For bold templates, use small preview cards for style previews and load the full `design.md` only after the user picks that template.
5. **Fixed 16:9 Stage (NON-NEGOTIABLE)**; Every deck uses a 1920×1080 slide canvas scaled as a whole to the viewport. Slides must stay 16:9 on every screen, including phones. Do not reflow slide content to fit the device.
- Never negate CSS functions directly (`-clamp()`, `-min()`, `-max()` are silently ignored); use `calc(-1 * clamp(...))` instead
- **Mode A: New Presentation**; Create from scratch. Go to Phase 1.
- **Mode B: PPT Conversion**; Convert a .pptx file. Go to Phase 4.
- **Mode C: Enhancement**; Improve an existing HTML presentation. Read it, understand it, enhance. **Follow Mode C modification rules below.**
**Question 1; Purpose** (header: "Purpose"):
**Question 2; Length** (header: "Length"):
**Question 3; Content** (header: "Content"):
**Question 4; Density** (header: "Density"):
- "Low density / speaker-led"; Big ideas, fewer words, more visual breathing room
- "High density / reading-first"; More self-contained detail for async reading
1. **Scan**; List all image files (.png, .jpg, .svg, .webp, etc.)
2. **Inspect each image**; Use the agent's available image-understanding capability. If image reading is unavailable, use filenames/metadata and ask the user to clarify only when needed
3. **Evaluate**; For each: what it shows, USABLE or NOT USABLE (with reason), what concept it represents, dominant colors
4. **Co-design the outline**; Curated images inform slide structure alongside text. This is NOT "plan slides then add images"; design around both from the start (e.g., 3 screenshots → 3 feature slides, 1 logo → title/closing slide)
**Logo in previews:** If a usable logo was identified, embed it (base64) into each style preview in Phase 2; the user sees their brand styled three different ways.
If images were provided, the slide outline already incorporates them from Step 1.2. If not, CSS-generated visuals (gradients, shapes, patterns) provide visual interest; this is a fully supported first-class path.
- [html-template.md](https://github.com/zarazhangrui/frontend-slides/tree/main/plugins/frontend-slides/skills/frontend-slides/html-template.md); HTML architecture and JS features
- [viewport-base.css](https://github.com/zarazhangrui/frontend-slides/tree/main/plugins/frontend-slides/skills/frontend-slides/viewport-base.css); Mandatory CSS (include in full)
- [animation-patterns.md](https://github.com/zarazhangrui/frontend-slides/tree/main/plugins/frontend-slides/skills/frontend-slides/animation-patterns.md); Animation reference for the chosen feeling
1. **Extract content**; Run `python scripts/extract-pptx.py <input.pptx> <output_dir>` (install python-pptx if needed: `pip install python-pptx`)
2. **Confirm with user**; Present extracted slide titles, content summaries, and image counts
3. **Style selection**; Proceed to Phase 2 for style discovery
4. **Generate HTML**; Convert to chosen style, preserving all text, images (from assets/), slide order, and notes only if explicitly requested; HTML comments remain visible in shared source
1. **Clean up**; Remove only task-created previews after checking ownership and whether the user needs them; preserve pre-existing files
2. **Open**; Use `open [filename].html` to launch in browser
3. **Summarize**; Tell the user:
- **Deploy to URL**; Shareable link that works on any device
- **Export to PDF**; Universal file for email, Slack, print
**Note:** Animations and interactivity are not preserved; the PDF is a static snapshot. This is normal and expected; mention it to the user so they're not surprised.
4. **Deliver the PDF**; The script auto-opens it. Tell the user:
   - That it works everywhere; email, Slack, Notion, Google Docs, print
- **Local images must be loadable via HTTP.** The script starts a local server and loads the HTML through it (so Google Fonts and relative image paths work). If images use absolute filesystem paths (e.g., `src="/Users/name/photo.png"`) instead of relative paths (e.g., `src="photo.png"`), they won't load. Generated presentations always use relative paths, but converted or user-provided decks might not; check and fix if needed.
- **Local images appear in the PDF** as long as they are in the same directory as (or relative to) the HTML file. The export script serves the HTML's parent directory over HTTP, so relative paths like `src="photo.png"` resolve correctly; including filenames with spaces. If images still don't appear, check: (1) the image files actually exist at the referenced path, (2) the paths are relative, not absolute filesystem paths like `/Users/name/photo.png`.
| [viewport-base.css](https://github.com/zarazhangrui/frontend-slides/tree/main/plugins/frontend-slides/skills/frontend-slides/viewport-base.css)             | Mandatory fixed-stage CSS; copy into every presentation             | Phase 3 (generation)      |