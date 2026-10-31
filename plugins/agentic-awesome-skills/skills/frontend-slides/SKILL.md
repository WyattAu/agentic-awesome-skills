1. **Zero Dependencies**; Single HTML files with inline CSS/JS. No npm, no build tools.
2. **Show, Don't Tell**; Generate visual previews, not abstract choices. People discover what they want by seeing it.
3. **Distinctive Design**; No generic "AI slop." Every presentation must feel custom-crafted.
4. **Viewport Fitting (NON-NEGOTIABLE)**; Every slide MUST fit exactly within 100vh. No scrolling within slides, ever. Content overflows? Split into multiple slides.
- ALL font sizes and spacing must use `clamp(min, preferred, max)`; never fixed px/rem
- Never negate CSS functions directly (`-clamp()`, `-min()`, `-max()` are silently ignored); use `calc(-1 * clamp(...))` instead
- **Mode A: New Presentation**; Create from scratch. Go to Phase 1.
- **Mode B: PPT Conversion**; Convert a .pptx file. Go to Phase 4.
- **Mode C: Enhancement**; Improve an existing HTML presentation. Read it, understand it, enhance. **Follow Mode C modification rules below.**
**Question 1; Purpose** (header: "Purpose"):
**Question 2; Length** (header: "Length"):
**Question 3; Content** (header: "Content"):
**Question 4; Inline Editing** (header: "Editing"):
- "Yes (Recommended)"; Can edit text in-browser, auto-save to localStorage, export file
- "No"; Presentation only, keeps file smaller
**Remember the user's editing choice; it determines whether edit-related code is included in Phase 3.**
1. **Scan**; List all image files (.png, .jpg, .svg, .webp, etc.)
2. **View each image**; Use the Read tool (Claude is multimodal)
3. **Evaluate**; For each: what it shows, USABLE or NOT USABLE (with reason), what concept it represents, dominant colors
4. **Co-design the outline**; Curated images inform slide structure alongside text. This is NOT "plan slides then add images"; design around both from the start (e.g., 3 screenshots → 3 feature slides, 1 logo → title/closing slide)
**Logo in previews:** If a usable logo was identified, embed it (base64) into each style preview in Phase 2; the user sees their brand styled three different ways.
- "Show me options" (recommended); Generate 3 previews based on mood
- "I know what I want"; Pick from preset list directly
- Impressed/Confident; Professional, trustworthy
- Excited/Energized; Innovative, bold
- Calm/Focused; Clear, thoughtful
- Inspired/Moved; Emotional, memorable
If images were provided, the slide outline already incorporates them from Step 1.2. If not, CSS-generated visuals (gradients, shapes, patterns) provide visual interest; this is a fully supported first-class path.
- [html-template.md](html-template.md); HTML architecture and JS features
- [viewport-base.css](viewport-base.css); Mandatory CSS (include in full)
- [animation-patterns.md](animation-patterns.md); Animation reference for the chosen feeling
1. **Extract content**; Run `python scripts/extract-pptx.py <input.pptx> <output_dir>` (install python-pptx if needed: `pip install python-pptx`)
2. **Confirm with user**; Present extracted slide titles, content summaries, and image counts
3. **Style selection**; Proceed to Phase 2 for style discovery
4. **Generate HTML**; Convert to chosen style, preserving supported text and images (from assets/) and slide order. Review tables, charts, grouped shapes, media and layout manually: this extractor does not reconstruct every PowerPoint object. Include speaker notes only in a separately agreed private deliverable
1. **Clean up**; Remove only preview files created for this task; preserve pre-existing files and previews the user wants to keep
2. **Open**; Use `open [filename].html` to launch in browser
3. **Summarize**; Tell the user:
| [viewport-base.css](viewport-base.css)             | Mandatory responsive CSS; copy into every presentation              | Phase 3 (generation)      |