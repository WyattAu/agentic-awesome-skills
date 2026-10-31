# Motion choreography: a generalizable animation guideline
separates intentional motion from jitter. **Framework-agnostic**; the rules hold for
## 1. When to animate: earn every transition
  they caused; not on a timer that plays at them.
## 2. Which transitions earn their keep: priority order
1. **Shared element**; the same object persists across views (thumbnail → hero). Says "this
2. **Reveal**; skeleton/loading → real content. Says "data loaded."
3. **List identity**; items keep identity as the set reorders/filters. Says "same items,
4. **State change**; something enters/exits (panel, toast, row). Says "this appeared/left."
5. **Route/section change**; moving to a new place.
## 3. Style by the *kind* of navigation: direction must be honest
| **Lateral / sibling** (tab ↔ tab, unordered) | **fade / cross-fade**; NOT a slide | there's no depth; a slide lies about it |
The single most common motion mistake is a directional slide on lateral navigation; it
  distance, trigger); bigger/further → a touch longer.
## 5. Mechanics: what to animate, and how (any stack)
- **Never `transition: all`**; list the exact properties; `all` silently animates
- **Correct `transform-origin`**; anchor motion where it "physically" starts (a menu from
- **Interruptible**; a new user input cancels/redirects the in-flight animation; motion is
- **SVG transforms**; apply to a `<g>` wrapper with `transform-box: fill-box;
  across a transition must NOT slide with the page content; give them their own transition
## 7. Accessibility: reduced motion