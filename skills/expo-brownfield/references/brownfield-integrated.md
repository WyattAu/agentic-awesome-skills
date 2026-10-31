Add React Native and Expo directly to the existing native project's build system; Gradle on Android, CocoaPods on iOS; the same way you would add any other library. The native project gains React Native capabilities while keeping a single, unified build.
- **Compatible Expo SDK and React Native versions**; verify every factory/lifecycle API against the selected SDK’s template. These imported examples mix historical versions; neither SDK 54 nor 55 alone proves compatibility.
- **Node.js (LTS)**; runs JavaScript and the Expo CLI.
- **Yarn**; manages JavaScript dependencies.
- **CocoaPods** (iOS); use the project’s supported Ruby/Bundler environment.
The new project ships a TypeScript example app. The JS entry point registers a root component under the name `"main"`; this name must match the `moduleName` referenced from the native side later.
Replace `'MyApp'` with the existing Xcode target name. The `:app_path` value tells `use_react_native!` where the JS app lives; set it to the absolute path of your Expo project root if you are in a monorepo.
Build and run the native app normally (Android Studio / Xcode). Navigate to your React Native–powered Activity or screen; it loads JS from the Metro dev server with hot reloading.
- **Development**; Metro serves the JS bundle with hot reloading over HTTP. Debug builds use the Metro URL via `RCTBundleURLProvider` (iOS) or the dev server detection in `ReactActivity` (Android).
- **Production**; Metro is not used. Run `expo export:embed` (invoked automatically by the React Native Gradle plugin and the iOS build phase) to embed the bundle into the APK/IPA.
- [./brownfield-isolated.md](./brownfield-isolated.md); Alternative: ship RN as a prebuilt AAR/XCFramework.
- [./comparison.md](./comparison.md); Decide between isolated and integrated.
- [./troubleshooting.md](./troubleshooting.md); Common Metro, build, and integration issues.