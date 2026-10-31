        // Outer shadow; detaches from background
        // Inner highlight (top-left); faked with overlay
- Use `.interpolatingSpring(stiffness: 300, damping: 10)` for the bouncy feel; critical for clay aesthetics.
- Use `Curves.elasticOut` or `Curves.bounceOut` for the spring animation; essential for the clay feeling.
        borderColor: 'rgba(255,255,255,0.3)', // Simplified; top highlight
- Gradient borders aren't possible natively; use a solid white-tinted border as a simplified approximation, or wrap in an `expo-linear-gradient` View.
- Use `spring(dampingRatio = 0.3f)`; low damping = bouncy. This is the core of the clay feeling.