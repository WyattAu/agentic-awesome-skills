This skill guides production-grade Android and cross-platform (non-iOS) app development following practices used at big tech companies. It covers the entire development lifecycle; architecture, UI, code quality, testing, error handling, release, and maintenance.
### Native Android: Kotlin + Jetpack Compose
### Native Android: Java + XML Views
- Java and Kotlin **coexist seamlessly** in the same project; migrate incrementally
1. **Color tokens**; Primary, secondary, surface, on-surface, error; light + dark variants
2. **Typography scale**; Display, headline, title, body, label (Material 3 type system)
3. **Spacing scale**; 4dp grid system (4, 8, 12, 16, 24, 32, 48dp)
4. **Shape tokens**; Corner radii per component family
5. **Component library**; Button, TextField, Card, BottomSheet, TopAppBar, etc.
- Back stack managed deliberately; don't push duplicates, use `popUpTo` / `launchSingleTop`
- No `!!` null assertions; use `?.let`, `?: return`, `requireNotNull` with message
- Never call methods on unchecked objects; null-check explicitly or use `Objects.requireNonNull`
- Use `ExecutorService` (not `AsyncTask`; deprecated) for background work; or `LiveData` + Room's built-in threading
- Use `ViewBinding`; never `findViewById`
- Null safety required; no `!` without explicit null check above
- No `any`; use `unknown` and narrow
- Avoid transitive dependency conflicts; use dependency resolution strategies
- Keep dependency count minimal; every added lib is a maintenance burden
- [ ] No hardcoded strings; use string resources / l10n
- `build.gradle.kts` only; no Groovy DSL in new projects
- Image loading: Coil (Compose) / Glide / Picasso; never load full-res in thumbnails
1. **Reproduce reliably**; document exact steps, device, OS version, account state
2. **Isolate**; is it UI, business logic, network, or persistence?
3. **Instrument**; add targeted logs / breakpoints, NOT shotgun logging
4. **Hypothesize**; form 1-3 specific hypotheses before touching code
5. **Fix the root cause**; never patch symptoms; trace back to the source
6. **Regression test**; write a test that fails before fix, passes after
7. **Document**; comment explaining why the fix works, not just what it does
- Never log PII; mask emails, phone numbers, tokens in logs
- Background restrictions vary widely by OEM; test push, alarms, background sync
### Phase 0: Foundation (Week 1-2)
### Phase 1: Core Features (Weeks 3-8)
### Phase 2: Polish (Weeks 9-12)
### Phase 3: Hardening (Weeks 12-14)
### Phase 4: Release
### Phase 5: Post-Launch (Ongoing)
- `references/native-android.md`; Kotlin, Compose, Room, Hilt, Coroutines
- `references/java-android.md`; Java, XML Views, ViewBinding, LiveData, Retrofit, Room, Hilt, migration path
- `references/flutter.md`; Dart, BLoC/Riverpod, Drift, go_router
- `references/react-native.md`; TypeScript, RN architecture, Hermes, New Architecture
- `references/kmm.md`; KMM shared modules, SQLDelight, Ktor, Compose Multiplatform
- `references/hybrid.md`; Capacitor, Ionic, PWA considerations