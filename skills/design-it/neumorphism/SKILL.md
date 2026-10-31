- The key trick: two `.shadow()` modifiers; one white (top-left), one dark (bottom-right).
    // Light shadow (top-left); iOS only supports one shadow
    // Android; use elevation for basic shadow
- Alternative: Wrap two nested `View`s; the outer one has the dark shadow, the inner one has the light shadow.
- Set the `Scaffold` background to the same `baseColor`; this is non-negotiable.