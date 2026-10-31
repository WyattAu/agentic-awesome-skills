A protocol for accessing web content in the most token-efficient, accurate, and structured way -
## Step 1: Classify the Input
### Step 1: Parse the URL
### Step 2: Use GitHub API (preferred over scraping)
Always prefer the GitHub API. It returns clean JSON; no HTML parsing needed.
# File tree (see what files exist: very cheap)
### Step 3: Layered Fetch for Repos
- README alone answers ~70% of "what does this repo do" questions; always try it first
### Step 1: Assess the URL type
### Step 2: Layered Fetch
Layer 1; Skim
Layer 2; Targeted Extract
Layer 3; Full Fetch
Layer 4; Browser Subagent (last resort only)
  → Do NOT use browser_subagent for static pages; it's expensive
Use when the user gives a topic, question, or query; not a specific URL.
### Step 1: Sharpen the Query Before Searching
### Step 2: Search and Select
3. Scan titles + snippets ONLY; do not fetch yet
### Step 3: Fetch Selected Results
Process results one at a time; only fetch the second URL if the first didn't answer the question.
5. Never dump raw content from multiple pages; always summarize per-source first
Confidence: [High / Medium / Low; based on source quality]