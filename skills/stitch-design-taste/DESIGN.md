## Configuration: Set Your Style
A restrained, gallery-airy interface with confident asymmetric layouts and fluid spring-physics motion. The atmosphere is clinical yet warm; like a well-lit architecture studio where every element earns its place through function. Density is balanced (Level 4), variance runs high (Level 8) to prevent symmetrical boredom, and motion is fluid but never theatrical (Level 6). The overall impression: expensive, intentional, alive.
- **Canvas White** (#F9FAFB); Primary background surface. Warm-neutral, never clinical blue-white
- **Pure Surface** (#FFFFFF); Card and container fill. Used with whisper shadow for elevation
- **Charcoal Ink** (#18181B); Primary text. Zinc-950 depth; never pure black
- **Steel Secondary** (#71717A); Body text, descriptions, metadata. Zinc-500 warmth
- **Muted Slate** (#94A3B8); Tertiary text, timestamps, disabled states
- **Whisper Border** (rgba(226,232,240,0.5)); Card borders, structural 1px lines. Semi-transparent for depth
- **Diffused Shadow** (rgba(0,0,0,0.05)); Card elevation. Wide-spreading, 40px blur, -15px offset. Never harsh
- **Emerald Signal** (#10B981); For growth, success, positive data dashboards
- **Electric Blue** (#3B82F6); For productivity, SaaS, developer tools
- **Deep Rose** (#E11D48); For creative, editorial, fashion-adjacent projects
- **Amber Warmth** (#F59E0B); For community, social, warm-toned products
- Purple/Violet neon gradients; the "AI Purple" aesthetic
- Pure Black (#000000); always Off-Black or Zinc-950
- **Display:** `Geist`, `Satoshi`, `Cabinet Grotesk`, or `Outfit`; Track-tight (`-0.025em`), controlled fluid scale, weight-driven hierarchy (700–900). Not screaming. Leading compressed (`1.1`). Alternatives forced; `Inter` is BANNED for premium contexts
- **Body:** Same family at weight 400; Relaxed leading (`1.65`), 65ch max-width, Steel Secondary color (#71717A)
- **Mono:** `Geist Mono` or `JetBrains Mono`; For code blocks, metadata, timestamps. When density exceeds Level 7, all numbers switch to monospace
- `Inter`; banned everywhere in premium/creative contexts
- Generic serif fonts (`Times New Roman`, `Georgia`, `Garamond`, `Palatino`); BANNED. If serif is needed for editorial/creative, use only distinctive modern serifs like `Fraunces`, `Gambarino`, `Editorial New`, or `Instrument Serif`. Never use default browser serif stacks. Serif is always BANNED in dashboards or software UIs regardless
* **Cards/Containers:** Generously rounded corners (`2.5rem`). Pure white fill. Whisper border (`1px`, semi-transparent). Diffused shadow (`0 20px 40px -15px rgba(0,0,0,0.05)`). Internal padding `2rem–2.5rem`. Used ONLY when elevation communicates hierarchy; high-density layouts replace cards with `border-top` dividers or negative space
The Hero is the first impression; it must be striking, creative, and never generic.
- **Inline Image Typography:** Embed small, contextual photos or visuals directly between words or letters in the headline. Example: "We build [photo of hands typing] digital [photo of screen] products"; images sit inline at type-height, rounded, acting as visual punctuation between words. This is the signature creative technique
- **Full-Height:** Use `min-height: 100dvh`; never `height: 100vh` (iOS Safari address bar jump)
Every screen must work flawlessly across all viewports. **Responsive is not optional; it is a hard requirement. Every single element must be tested at 375px, 768px, and 1440px.**
> **Note:** Stitch generates static screens; it does not animate. This section documents the **intended motion behavior** so that the coding agent (Antigravity, Cursor, etc.) knows exactly how to implement animations when building the exported design into a live product.
- **Perpetual Micro-Loops:** Only essential status feedback may animate, with reduced-motion and pause/stop behavior; do not give every component an infinite-loop state; Pulse on status dots, Typewriter on search bars, Float on feature icons, Shimmer on loading states
- No emojis; anywhere in UI, code, or alt text
- No `Inter` font; use `Geist`, `Outfit`, `Cabinet Grotesk`, `Satoshi`
- No generic serif fonts (`Times New Roman`, `Georgia`, `Garamond`); if serif is needed, use distinctive modern serifs only (`Fraunces`, `Instrument Serif`)
- No pure black (`#000000`); Off-Black or Zinc-950 only
- No overlapping elements; text never overlaps images or other content. Clean spatial separation always
- No filler UI text: "Scroll to explore", "Swipe down", "Discover more below", scroll arrows, bouncing chevrons; all BANNED
- No broken Unsplash links; use `picsum.photos/seed/{id}/800/600` or SVG UI Avatars
- No generic `shadcn/ui` defaults; customize radii, colors, shadows to match this system
- No `z-index` spam; use only for Navbar, Modal, Overlay layer contexts
- No `h-screen`; always `min-h-[100dvh]`
- No circular loading spinners; skeletal shimmer only