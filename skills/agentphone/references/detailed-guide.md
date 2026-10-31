2. You create an **Agent**; this is the AI persona that handles calls and messages
└── Agent (AI persona; owns numbers, handles calls/SMS)
- **`hosted`**; The built-in LLM handles the conversation autonomously using the agent's `system_prompt`. No server required. This is the easiest way to get started; just set a prompt and make a call.
- **`webhook`** (default); Inbound call/SMS events are forwarded to your webhook URL for custom handling. Use this when you need full control over the conversation logic.
Only provided fields are updated; everything else stays the same.
| `areaCode` | `string` | No |; | 3-digit area code (US/CA only) |
| `agentId` | `string` | No |; | Attach to an agent immediately |
**Irreversible**; the number returns to the carrier pool and you cannot get it back. Always confirm with the user before releasing.
- **`voiceMode: "webhook"`** (default); Caller speech is transcribed and sent to your webhook as `agent.message` events. Your server controls every response using any LLM, RAG, or custom logic.
- **`voiceMode: "hosted"`**; Calls are handled end-to-end by a built-in LLM using your `systemPrompt`. No webhook or server needed.
1. **Caller dials your number**; The voice engine answers and begins streaming audio.
2. **Caller speaks**; Streaming STT transcribes in real-time and detects end of speech.
3. **Transcript is sent to your webhook**; We POST the transcript to your webhook with `event: "agent.message"` and `channel: "voice"`, including `recentHistory` for context.
4. **Your server responds**; You process the transcript (e.g., send to your LLM) and return a response. We strongly recommend streaming NDJSON; TTS starts speaking on the first chunk.
5. **TTS speaks the response**; Each NDJSON chunk is spoken with sub-second latency. No waiting for the full response.
6. **Conversation continues**; The caller can interrupt at any time (barge-in). The cycle repeats naturally.
1. **Caller dials your number**; The AI answers with your `beginMessage` (e.g., "Hello! How can I help?").
2. **Caller speaks**; Streaming STT transcribes in real-time.
3. **Built-in LLM generates a response**; The LLM uses your `systemPrompt` to generate a contextual response.
4. **TTS speaks the response**; Streaming TTS speaks the response with sub-second latency.
5. **Conversation continues**; No server or webhook involved; the platform handles everything.
| Call recording       | Optional add-on; automatically records calls and provides audio URLs |
Mark interim chunks with `"interim": true`; the final chunk (without `interim`) closes the turn. Use this for tool calls, LLM token forwarding, or any time your response takes more than ~1 second.
| `interim` | boolean | NDJSON only; marks a chunk as interim (TTS speaks it but the turn stays open)                                                                            |
> **Warning: Webhook timeout**; Voice webhook requests have a **30-second default timeout** (configurable from 5–120 seconds per webhook via the `timeout` field). If your server doesn't start responding in time, the request is cancelled and the caller hears silence for that turn. This is especially important when your webhook calls external APIs or runs LLM tool calls; always stream an interim chunk immediately so the caller hears something while you process.
> **Tip: Why interim chunks matter for tool calls**; Without the interim chunk, the caller hears dead silence while your LLM decides which tool to call, the external API responds, and the LLM summarises the result. With streaming, they hear "Let me check on that" within milliseconds; just like a human assistant would.
**You're returning a `200 OK` with no body, or a non-JSON response for voice.** SMS webhooks only need a `200` status; voice webhooks must return a JSON object with a `text` field.
| `status`    | string  | No       |;       | Filter by status: `completed`, `in-progress`, `failed`      |
| `direction` | string  | No       |;       | Filter by direction: `inbound`, `outbound`, `web`           |
| `search`    | string  | No       |;       | Search by phone number (matches `fromNumber` or `toNumber`) |
| `url` | `string` | Yes |; | Publicly accessible HTTPS URL |
**Save the `secret`**; use it to verify webhook signatures on your server.
| `agent.message` | Real-time voice transcript or SMS received; check `channel` field |
- **`channel: "voice"`**; Real-time voice call event. Your response **must** be a JSON object with a `text` field (e.g. `{"text": "Hello!"}`). Return `Content-Type: application/x-ndjson` for streaming responses. Non-object responses are ignored and the caller hears silence.
- **`channel: "sms"`**; SMS message event. A `200 OK` status is sufficient; no response body needed.
- **Appointment scheduling**; Call businesses to book appointments on your human's behalf. Handle the back-and-forth conversation autonomously.
- **Customer support hotline**; Set up an agent with a system prompt that knows your product. It handles inbound calls 24/7.
- **Outbound sales calls**; Make calls to leads with a tailored pitch. Check transcripts to see how each call went.
- **SMS notifications**; Send appointment reminders, order updates, or alerts to your users via SMS.
- **Phone verification**; Call or text users to verify their phone numbers during signup.
- **IVR replacement**; Replace clunky phone trees with a conversational AI that understands natural language.
- **Meeting reminders**; Call or text participants before meetings to confirm attendance.
- **Lead qualification**; Call inbound leads, ask qualifying questions, and log the results.
- **Personal assistant**; Give your AI a phone number so it can handle calls and texts on your behalf; scheduling, reminders, and follow-ups.