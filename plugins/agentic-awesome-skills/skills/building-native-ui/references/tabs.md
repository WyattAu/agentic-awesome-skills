- Tabs must be static; no dynamic addition/removal at runtime (remounts navigator, loses state)
// Xcode asset catalog; iOS only (SDK 55+)
// Rendering mode; iOS only (SDK 55+)
**Important**: Two instances render simultaneously; store state outside the component (props, context, or external store).