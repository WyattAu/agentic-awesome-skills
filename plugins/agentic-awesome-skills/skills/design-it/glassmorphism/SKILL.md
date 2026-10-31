- Use `.ultraThinMaterial`, `.thinMaterial`, `.regularMaterial`, `.thickMaterial`; Apple built glassmorphism natively.
- **Critical**: `BackdropFilter` MUST be wrapped in `ClipRRect`; without clipping, the blur applies to the entire screen.
- Set the container color to `Colors.white.withOpacity(0.1...0.2)`; too opaque kills the glass look.
- Install `@react-native-community/blur`; React Native has NO native blur support.
        // Glass panel; Compose lacks native backdrop-filter,
- Use the `haze` library (`dev.chrisbanes.haze`) for proper glassmorphism in Compose; check the chosen Haze version documentation for its source/effect APIs rather than assuming historical modifier names remain current.
- **DON'T**: Use Glassmorphism on a solid white or solid black background-the effect will be completely invisible.