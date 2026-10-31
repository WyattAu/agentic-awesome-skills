description: Get your project to 100% ✪ AI-readiness, fast; the AI auto-detects your stack and only asks for what it can't know (your goal and the human "why"). Least typing, maximum context. For time-conscious builders; feeds into faf-expert for depth.
# FAF Context: Give the AI What It Needs
Use this skill when you need get your project to 100% ✪ AI-readiness, fast; the AI auto-detects your stack and only asks for what it can't know (your goal and the human "why"). Least typing, maximum context. For time-conscious builders; feeds into faf-expert for depth.
**AI writes its best code when it has your project's context. This skill helps you hand it over; fast, and to 100%.**
`.faf` is an **IANA-registered context format** (`application/vnd.faf+yaml`); a typed, portable file *you own*, readable by any AI (no bespoke manifest, no vendor lock-in). The whole point is one number: **AI-readiness, 0–100%.** At **100% ✪** the AI starts every session already knowing your project; no re-explaining, no guessing. This skill is the *builder's* path to that number: minimum typing, maximum context.
**faf-cli has 21 slots.** It works in three steps; and only the last one needs you:
1. **Your app type sets which slots are *required*.** A CLI needs different slots than a full-stack web app; faf-cli right-sizes the set and `slotignored`s the rest (never counted against you).
3. **Whatever's left empty, the AI asks you.** Those are the bits only you know; usually a couple of the 6 Ws (often *why* and *when*). Answer them → **100% ✪.**
> *(Teams / Enterprise tiers add more slots; monorepos, caching, versioning; but those aren't faf-cli. **faf-cli is the 21.**)*
## You rarely type all 6 Ws: here's why
The 6 Ws are "the underivable half"; but you almost never write all six from scratch:
> **"Sometimes 3 Ws is enough. Sometimes the goal alone is enough."** A great goal sentence + auto-detection can carry who/what/where/how on their own; leaving you two small answers. The better your goal, the less you type.
The goal is the **generative input**; it seeds who/what/where automatically. Make it a real, specific sentence (it's also your *use-case*):
- ✅ *"A CLI that scores any repo's AI-readiness and syncs context to Claude, Cursor, and Gemini; for solo developers."*
- ❌ *"A tool to improve development."* → generic; seeds nothing. (Generic phrases are *ignored* on purpose; empty beats wrong.)
## The 6 Ws: terse labels, not prose
Each W is a **3–4 word label** (hard cap < 6); a scannable spec card, not a paragraph:
A CLI has no frontend; an API has no UI library. Mark those **`slotignored`** and they drop out of the denominator; you're scored only on slots that *matter for your app type*. **100% means "everything that applies is filled,"** not "every box checked." (`faf auto` and `faf go` handle most of this for you.)
The AI **only seeds facts your goal/README literally state**; never invents, never uses templates. What it can't source, it leaves empty for you. **Empty beats wrong.** That's why the resulting context is trustworthy: every slot is either detected, stated by you, or honestly blank.
**The goal:** the AI is only as good as the context you give it. Answer the few things only you know; the gaps it couldn't fill itself; and it's optimized to help you at **100% ✪**.