description: "Relationship-aware daily gift engine with five-stage creative pipeline; editorial judgment, synthesis, concept generation, visual strategy, and rendering in H5, image, or video"
A relationship-aware gift engine that decides *whether* a gift should exist before deciding *what* it should be. Uses a five-stage creative pipeline to generate personalized daily gifts in H5 (interactive web pages), AI-generated images, or AI-generated videos. The core design principle is "idea before medium"; the creative concept is locked before the output format is chosen.
Extract six content slots from conversation context (today_theme, emotion_peaks, historical_echo, open_loop, lobster_judgment, preference_hint). Form a gift thesis = anchor (which moment deserves the center) + return (what new perspective the agent gives back). If the thesis has no return, it's not a gift; it's a decorated log entry.
- **Multi-layer anti-repetition**: concept family, visual elements, theme, style, content direction; each tracked across sliding windows of recent gifts
- **Three-layer user taste profile**: Layer 1 (identity; stable), Layer 2 (context; updates every 5-7 gifts), Layer 3 (signals; auto-appended after every gift)
- ✅ Let the editorial judgment decide; not every day needs a gift
- ❌ Don't default to "reflect on today" every time; vary content direction
- Requires API keys for image/video generation (optional; H5 works without them)
- The skill reads conversation context and memory files to inform editorial judgment; this is core to personalization but means it has broad read access within the agent's workspace.
- Image generation skills; for standalone image creation without the gift pipeline
- Cron/scheduling skills; for understanding the daily trigger mechanism