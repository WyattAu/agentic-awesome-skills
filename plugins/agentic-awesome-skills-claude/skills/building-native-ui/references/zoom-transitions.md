Coordinates are in the destination screen's coordinate space. Prefer `Link.AppleZoomTarget` when possible; use `alignmentRect` only when the target element isn't available as a React component.
This is useful when the destination contains a zoomable scroll view; the system gives that scroll view precedence over the dismiss gesture.
- Skinny full-width list rows as zoom sources; the transition looks unnatural
- Using zoom with sheets or popovers; only works in Stack navigator
- Hiding the navigation bar; known issues with header visibility during transitions
- Always provide a close or back button; dismissal gestures are not discoverable
- Source view doesn't need to match the tap target; only the `Link.AppleZoom` wrapped element animates