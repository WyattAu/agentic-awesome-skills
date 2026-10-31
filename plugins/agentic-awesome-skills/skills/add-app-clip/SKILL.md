- `targets/clip/expo-target.config.js`; the target's config plugin
- `targets/clip/Info.plist`; Clip Info.plist
Pick a good icon or reuse the existing one defined in the app; check it with `bunx expo config` under the `icon` or `ios.icon` key.
- `activitycontinuation` is optional and used for sharing the link between mobile and desktop. Must be used with `Head` from expo-router; see https://docs.expo.dev/router/advanced/apple-handoff/
Look at the `infoPlist` object; mirror the permission keys in the App Clip's `Info.plist` so matching APIs can be used from the Clip.
Set `deploymentTarget: "17.6"` in the Clip's target config; App Clips have a higher minimum size limit in iOS 17.6.
3. Sync capabilities; note `Enabled: Associated Domains` for the Clip target.
- ./references/native-module.md; Local Expo module to detect App Clip context and present the SKOverlay install prompt