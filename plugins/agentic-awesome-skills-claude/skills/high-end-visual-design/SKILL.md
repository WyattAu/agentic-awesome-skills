**Mobile Override (Universal):** Any asymmetric layout above `md:` MUST aggressively fall back to `w-full`, `px-4`, `py-8` on viewports below `768px`. Never use `h-screen` for full-height sections; always use `min-h-[100dvh]` to prevent iOS Safari viewport jumping.
- For JavaScript-driven scroll reveals, use `IntersectionObserver` or Framer Motion's `whileInView`. Never use `window.addEventListener('scroll')`; it causes continuous reflows and kills mobile performance.
- **Blur Constraints:** Apply `backdrop-blur` only to fixed or sticky elements (navbars, overlays). Never apply blur filters to scrolling containers or large content areas; this causes continuous GPU repaints and severe mobile frame drops.
- [ ] Section padding is at minimum `py-24`; the layout breathes heavily
- [ ] All transitions use custom cubic-bezier curves; no `linear` or `ease-in-out`
- [ ] Scroll entry animations are present; no element appears statically
- [ ] All animations use only `transform` and `opacity`; no layout-triggering properties