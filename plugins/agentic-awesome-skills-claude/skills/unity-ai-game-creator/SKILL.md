This skill transforms a raw game concept into a fully structured Unity development plan with specifications for assets, scenes, music, scripts and build validation. A plan or prompt kit is not a completed Unity project or verified build. It guides the agent through a 5-phase pipeline; from extracting core game dimensions to producing ready-to-use AI prompts for every asset category; using the latest Unity 6+ features and AI tooling ecosystem. Unlike generic Unity reference skills, this skill is workflow-driven: the user provides an idea, and the agent delivers a complete, actionable game development roadmap.
Extract and clarify these dimensions from the user's game idea. Ask only for what is ambiguous; infer the rest from context.
1. **Top 3 Reference Games**; What they do well, what the user can learn
2. **Market Gap Hypothesis**; What opportunity to research; distinguish evidence from assumptions
3. **Core Differentiator**; The one unique thing about this game
4. **Risk Assessment**; Technical and market risks with mitigations
1. **Executive Summary**; Elevator pitch, genre, USPs
2. **Gameplay**; Core loop diagram, progression, abilities, win/loss, difficulty curve
3. **World & Narrative**; Setting, characters, level structure
4. **Art Direction**; Visual style, color palette (hex codes), reference descriptions, UI/UX style
5. **Audio Direction**; Music style per scene, SFX categories, voice needs
6. **Technical Spec**; Unity version, render pipeline, performance budgets, SDKs
7. **Monetization Strategy**; Revenue model, IAP design, ad placement
8. **Development Roadmap**; Phases, milestones, feature priority matrix, risk register
- **Environment**; Ground/terrain, skybox, lighting setup, props with positions
- **Interactive Objects**; Behavior on interaction
- **Characters / NPCs**; AI behavior, patrol paths, dialogue triggers
- **UI Overlay**; HUD elements, contextual prompts
- **Audio Layers**; Background music (mood, tempo, loop point), ambient SFX, trigger SFX
- **Camera Setup**; Type (Cinemachine/Fixed/Follow), behavior configuration
- **Systems Active**; Save checkpoints, spawners, particle effects
1. **Foundation**; Project setup, GameManager, event system, scene management, input config
2. **Core Gameplay**; Player controller, camera, core loop, basic enemies
3. **Content & Systems**; Level building, UI system, audio manager, save/load
4. **Polish & Feedback**; VFX/juice, progression, tutorial, settings
5. **Platform & Release**; Profiling, monetization, analytics, platform polish, submission
App icon, feature graphic, screenshots, promotional video, descriptions, keywords, privacy policy, age rating, content rating; with AI prompts for generating marketing assets.
1. **Game Brief**; Cozy farm sim × magical creatures, mobile portrait, stylized low-poly, 5-min sessions, F2P with cosmetic IAP
2. **3 Reference Games**; Stardew Valley (loop), Merge Magic (mobile UX), Moonstone Island (magic farm blend)
3. **Core Loop**; Plant → Tend → Harvest → Sell → Upgrade → Discover magical seeds
4. **Scene Blueprints**; Farm (main), Village Market, Enchanted Forest, Player Home
5. **AI Asset Kit**; 15 crop models, 8 building models, 5 character models, seasonal music tracks, ambient farm SFX, UI kit
6. **Technical Plan**; Unity 6 LTS + URP, mobile-first 30fps, addressable assets for seasonal updates
7. **6-Week Roadmap**; Foundation → Core Gameplay → Content → Monetization → Polish → Soft Launch
1. **Game Brief**; Arena FPS, PC/Steam, stylized neon cyberpunk, 10-min matches, Premium $9.99
2. **Core Loop**; Spawn → Loot → Fight → Eliminate → Score → Respawn
3. **Technical Plan**; Unity 6 + HDRP, Netcode for GameObjects, dedicated servers, 120fps target
4. **AI Asset Kit**; Arena models, weapon models, neon material/shader prompts, electronic music tracks, weapon SFX
1. **Game Brief**; Casual puzzle, mobile portrait, flat minimalist, 2-min sessions, F2P with rewarded ads
2. **Scope**; Prototype in 1 week, MVP in 3 weeks
3. **Scene Blueprints**; Main Menu, Game Board, Results Screen, Daily Challenge
4. **AI Asset Kit**; UI sprites, celebration particles, calm ambient music, tap/swipe SFX
- ✅ **Start with the core loop**; Nail the 30-second cycle before anything else
- ✅ **Profile before optimizing**; Use Unity Profiler to find real bottlenecks
- ✅ **Use ScriptableObjects for data**; Decouple data from logic for flexibility
- ✅ **Generate multiple AI asset variations**; Only within the approved call/cost budget; inspect topology, rig, scale, PBR maps and rights rather than assuming prompt constraints were met
- ✅ **Test on real devices early**; Emulators miss platform-specific issues
- ✅ **Version control from day one**; Git + LFS, commit often
- ❌ **Don't hardcode tool choices**; Always present alternatives to the user
- ❌ **Don't skip the GDD**; Even solo projects benefit from written design
- ❌ **Don't optimize prematurely**; Make it work, make it right, make it fast
- ❌ **Don't ship AI assets without review**; Check licensing terms for each tool
- Performance budgets are guidelines; always profile on actual target hardware.
- AI tools and their capabilities evolve rapidly; verify current availability and pricing before recommending.
- Asset generation prompts are text-only guidance; no automated downloads or file mutations occur.
- **Problem:** Scope creep; trying to build everything at once