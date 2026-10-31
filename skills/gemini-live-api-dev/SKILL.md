- **Bidirectional audio streaming**; real-time mic-to-speaker conversations
- **Video streaming**; send camera/screen frames alongside audio
- **Text input/output**; send and receive text within a live session
- **Audio transcriptions**; get text transcripts of both input and output audio
- **Voice Activity Detection (VAD)**; automatic interruption handling
- **Native audio**; thinking (with configurable `thinkingLevel`)
- **Function calling**; synchronous tool use
- **Google Search grounding**; ground responses in real-time search results
- **Session management**; context compression, session resumption, GoAway signals
- **Ephemeral tokens**; secure client-side authentication
- `gemini-3.1-flash-live-preview`; Optimized for low-latency, real-time dialogue. Native audio output, thinking (via `thinkingLevel`). 128k context window. **This is the recommended model for all Live API use cases.**
- `gemini-3.5-live-translate-preview`; Real-time streaming translation model.
> - `gemini-2.5-flash-native-audio-preview-12-2025`; Migrate to `gemini-3.1-flash-live-preview`.
> - `gemini-live-2.5-flash-preview`; Released June 17, 2025. Shutdown: December 9, 2025.
> - `gemini-2.0-flash-live-001`; Released April 9, 2025. Shutdown: December 9, 2025.
- **Python**: `google-genai`; `pip install google-genai`
- **JavaScript/TypeScript**: `@google/genai`; `npm install @google/genai`
- [LiveKit](https://docs.livekit.io/agents/models/realtime/plugins/gemini/); Use the Gemini Live API with LiveKit Agents.
- [Pipecat by Daily](https://docs.pipecat.ai/guides/features/gemini-live); Create a real-time AI chatbot using Gemini Live and Pipecat.
- [Fishjam by Software Mansion](https://docs.fishjam.io/tutorials/gemini-live-integration); Create live video and audio streaming applications with Fishjam.
- [Vision Agents by Stream](https://visionagents.ai/integrations/gemini); Build real-time voice and video AI applications with Vision Agents.
- [Voximplant](https://voximplant.com/products/gemini-client); Connect inbound and outbound calls to Live API with Voximplant.
- [Firebase AI SDK](https://firebase.google.com/docs/ai-logic/live-api?api=dev); Get started with the Gemini Live API using Firebase AI Logic.
        # Audio: process ALL parts in each event
- `gemini-3.5-live-translate-preview`; The recommended translation model for all Live Translate use cases.
- **Response modality**; Only `TEXT` **or** `AUDIO` per session, not both. Native audio models only support audio.
- **Audio-only session**; 15 min without compression
- **Audio+video session**; 2 min without compression
- **Connection lifetime**; ~10 min (use session resumption)
- **Context window**; 128k tokens (native audio) / 32k tokens (standard)
- **Async function calling**; Not yet supported; function calling is synchronous only. The model will not start responding until you've sent the tool response.
- **Proactive audio**; Not yet supported in Gemini 3.1 Flash Live. Remove any configuration for this feature.
- **Affective dialogue**; Not yet supported in Gemini 3.1 Flash Live. Remove any configuration for this feature.
- **Code execution**; Not supported
- **URL context**; Not supported
1. **Model string**; Update from `gemini-2.5-flash-native-audio-preview-12-2025` to `gemini-3.1-flash-live-preview`.
2. **Thinking configuration**; Use `thinkingLevel` (`minimal`, `low`, `medium`, `high`) instead of `thinkingBudget`. Default is `minimal` for lowest latency.
3. **Server events**; A single event can contain multiple content parts simultaneously (audio + transcript). Process **all** parts in each event.
4. **Client content**; `send_client_content` is only for seeding initial context history (set `initial_history_in_client_content` in `history_config`). Use `send_realtime_input` for text during conversation.
5. **Turn coverage**; Defaults to `TURN_INCLUDES_AUDIO_ACTIVITY_AND_ALL_VIDEO` instead of `TURN_INCLUDES_ONLY_ACTIVITY`. If sending constant video frames, consider sending only during audio activity to reduce costs.
6. **Async function calling**; Not yet supported. Function calling is synchronous only.
7. **Proactive audio & affective dialogue**; Not yet supported. Remove any configuration for these features.
4. **Use ephemeral tokens** for client-side deployments; never expose API keys in browsers
8. **Process all parts** in each server event; events can contain multiple content parts
- [Live API Overview](https://ai.google.dev/gemini-api/docs/live.md.txt); getting started, raw WebSocket usage
- [Live Translate](https://ai.google.dev/gemini-api/docs/live-api/live-translate.md.txt); configuration options and capabilities for translation
- [Live API Capabilities Guide](https://ai.google.dev/gemini-api/docs/live-guide.md.txt); voice config, transcription config, native audio (thinking), VAD configuration, media resolution
- [Live API Tool Use](https://ai.google.dev/gemini-api/docs/live-tools.md.txt); function calling (sync and async), Google Search grounding
- [Session Management](https://ai.google.dev/gemini-api/docs/live-session.md.txt); context window compression, session resumption, GoAway signals
- [Ephemeral Tokens](https://ai.google.dev/gemini-api/docs/ephemeral-tokens.md.txt); secure client-side authentication for browser/mobile
- [WebSockets API Reference](https://ai.google.dev/api/live.md.txt); raw WebSocket protocol details