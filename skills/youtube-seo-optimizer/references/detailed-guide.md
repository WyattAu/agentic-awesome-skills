You need web search + URL fetch for this to work. Whatever your host calls them; `web_search`/`web_fetch`, `WebSearch`/`WebFetch`, an MCP tool; use those.
1. **Read the user's request**; scope check first (see below)
2. **Steps 0-2**; Classify, find the keyword, research it, gather missing info
## Step 0: Check scope then classify
**Scope check first.** This skill handles one video, podcast episode, or short-form clip at a time; not entire channels or playlists. If the user asks for something outside that, say: *"I can optimize individual videos or episodes. Which one should I start with?"* Once they pick one, restart the flow from Step 0 with that specific item.
**Then classify.** Read the user's message first; they might have already told you everything. Don't ask something they just said.
**Content type**; If unclear from what they said, ask once: *"Is this a regular video, a podcast episode, or a Short/Reel?"*
**Status**; Did they give a URL? Fetch it. Did they say "no views" or "not ranking"? It's existing. Did they say "uploading" or "about to post"? It's new. If you can't tell, ask once.
If they gave a URL, fetch the live metadata; don't ask them to repeat what's already there.
## Step 1: Find the target keyword
If the user asked for just a title or just tags, skip the full research batch; but still do one quick search to validate the keyword angle. Otherwise run the full research:
- `[target keyword]`; see what's ranking
- `[niche/topic] trending` or `[target keyword] reddit`; real phrasing people use
**Verify superlatives.** If they say "top 10," "#1," "fastest-growing"; search for proof. If unverified, drop it or mark `[VERIFY: ...]`.
## Step 2: Gather what you still need (scrape first, ask last)
One question at a time. After each answer, see if you can fill the rest from what you learned. If they say "placeholders," use `[ADD: ...]` markers; never fake URLs.
- First 3 hashtags surface below the title; choose strategically
- Placement: final line of the description only; never in the tags field
- Don't reuse an identical set across every upload; vary 3-6 per video
### Block 1: Hook (first ~150 characters, shown in search results)
### Block 2: Body
- Weave in secondary keywords; one per paragraph, naturally
### Block 3: Footer
[Hook; target keyword in sentence 1. Core promise. Who this is for.]
[Body paragraph; secondary keyword woven in naturally]
[Body paragraph; secondary keyword woven in naturally]
[Body paragraph; geo signal if applicable]
[Body paragraph; guest bio (podcast) or credentials (video)]
[Podcast; 🎧 Listen on: Spotify | Apple Podcasts | ...]
[Subscribe CTA; 2 sentences, includes channel/show name]
- First chapter MUST be `0:00`; YouTube ignores all chapters without it
If they ask for a Short without mentioning a source video, ask: *"Which video should I pull the Short from?"*; once they tell you, treat the Short as the primary request and use Mode E directly (no need to also package the source video).
- **Description:** 150-200 words; hook + hashtags, no chapters
## Mode A: New Video Upload Package
📺 YOUTUBE SEO PACKAGE; NEW UPLOAD
[Title; 60-70 characters, built around Step 1 target keyword]
[Alternative title; different hook, same keyword]
[Full description; see Description Structure section]
"[2-3 sentences; natural speech, next topic + subscribe]"
## Mode B: Existing Video Audit + Fix
- YouTube URL (preferred; fetch live metadata) or current title/description
SECTION 1; AUDIT SCORECARD
SECTION 2; DETAILED FINDINGS
SECTION 3; FULL REWRITTEN METADATA
[New title; 60-70 chars]
[Alternative title; different hook]
SECTION 4; POST-FIX ACTION PLAN
Step 1; Do immediately (YouTube Studio):
Step 2; Within 48 hours:
Step 3; Check in 7 days:
## Mode C: New Podcast Episode Package
🎙️ PODCAST EPISODE SEO PACKAGE; NEW EPISODE
[Title; 60-70 chars. Lead with guest name if searchable, else keyword.]
[Full description; guest bio in Block 2, platform links in Block 3]
[tag1], [tag2] ... [tag14-19; include show + guest name]
"[2-3 sentences; thank guest, tease next, subscribe]"
## Mode D: Existing Podcast Episode Audit + Fix
- YouTube URL (preferred; fetch live metadata) or current title/description
SECTION 1; SCORECARD
SECTION 2; DETAILED FINDINGS
SECTION 3; FULL REWRITTEN METADATA
[New title; 60-70 chars]
SECTION 4; ACTION PLAN
Step 1; Do immediately (YouTube Studio):
Step 2; Within 48 hours:
Step 3; Check in 7 days:
## Mode E: New Short-Form / Reel Package
🎬 SHORT-FORM SEO PACKAGE; NEW SHORT / REEL / CLIP
[Title; 60-70 chars. Keyword in first 3 words. One power word/hook.]
⑦ CROSS-POST CAPTION (Reels / TikTok; if cross-posting)
[1 sentence; "full episode linked above", "part 2 tomorrow", etc.]
## Mode F: Existing Short-Form Audit + Fix
SECTION 1; SCORECARD
SECTION 2; DETAILED FINDINGS
SECTION 3; REWRITTEN METADATA
SECTION 4; ACTION PLAN
Step 1; Do immediately:
Step 2; Check in 7 days:
- **Only say what they need**; don't dump sections they didn't request
## Quality Checklist: run before sending
- [ ] Step 1 search batch run; secondary keywords from real results