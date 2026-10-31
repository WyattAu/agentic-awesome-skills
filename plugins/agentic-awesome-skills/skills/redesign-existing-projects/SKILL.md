1. **Scan**; Read the codebase. Identify the framework, styling method (Tailwind, vanilla CSS, styled-components, etc.), and current design patterns.
2. **Diagnose**; Use the audit as design options, not universal defects. Prioritize evidenced usability/accessibility issues and the user’s brand goals; a familiar font, sidebar or symmetric grid can be appropriate.
3. **Fix**; Apply targeted upgrades working with the existing stack. Do not rewrite from scratch. Improve what's there.
- **Random dark sections in a light mode page (or vice versa).** A single dark-background section breaking an otherwise light page looks like a copy-paste accident. Either commit to a full dark mode or keep a consistent background tone throughout. If contrast is needed, use a slightly darker shade of the same palette; not a sudden jump to `#111` in the middle of a cream page.
- **Empty, flat sections with no visual depth.** Sections that are just text on a plain background feel unfinished. Add high-quality background imagery (blurred, overlaid, or masked), subtle patterns, or ambient gradients. Use reliable placeholder sources like `https://picsum.photos/seed/{name}/1920/1080` when real assets are not available. Experiment with background images behind hero sections, feature blocks, or CTAs; even a subtle full-width photo at low opacity adds presence.
- **Symmetrical vertical padding.** Top and bottom padding are always identical. Adjust optically; bottom padding often needs to be slightly larger.
- **Broken grid / asymmetry.** Elements that deliberately ignore column structure; overlapping, bleeding off-screen, or offset with calculated randomness.
1. **Font swap**; biggest instant improvement, lowest risk
2. **Color palette cleanup**; remove clashing or oversaturated colors
3. **Hover and active states**; makes the interface feel alive
4. **Layout and spacing**; proper grid, max-width, consistent padding
5. **Replace generic components**; swap cliche patterns for modern alternatives
6. **Add loading, empty, and error states**; makes it feel finished
7. **Polish typography scale and spacing**; the premium final touch