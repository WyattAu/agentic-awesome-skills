# De-AI Writer: Chinese AI-Smell Removal
This skill ships the pattern catalog plus the editing procedure: score a draft for AI smell, rewrite it against the specific patterns it hits, clone a reference style, and review the result. The full 35-pattern catalog lives in `references/ai-patterns-zh.md` and is plain Markdown; usable as a prompt by any assistant.
*Note on the engine: this bundle is documentation only. A zero-dependency local rule engine is published in the source repository (see `source_repo`) as an optional external prerequisite; it is not included here.*
Scan the text, record which patterns hit *and where*, then compute the AI-smell index with the deterministic formula below. Do not rewrite from vibes; the same phrases recur, and you need the hit list to verify the edit afterwards.
#### The AI-smell index (deterministic: the same formula must be used before and after)
1. **Count hits per paragraph.** For each pattern, count one hit per paragraph; repeats inside the same paragraph do not inflate the score.
3. **Normalize by length.** `D = 加权总分 / max(1, 总字数 / 100)`; weighted hits per 100 characters.
6. **Report three numbers, not one:** 命中处数 / 加权总分 / AI味指数. After rewriting, recompute with the same formula so the delta is comparable. If a hit cannot be attributed to a catalogued pattern, report only the observable hit count and say the index is not computed; never invent a number.
- **A single hit is not evidence.** The catalog marks certain patterns (em-dash, hedges, passive voice, 的-stacking, quotation marks) as 弱证据; only act when two or more appear in the same paragraph.
**The facts come from the source, never from the pattern list.** A rewrite may delete packaging, reorder, and rephrase; it must not introduce a fact the source does not contain. If the rewrite would be clearer with a number or a specification (rotational speed, battery life, materials), and the source has none, ask the user for it; never supply one.
Re-score the rewritten text with the same Step 1 formula. The index should drop and the semantic content must be preserved; report the before/after triple (命中处数 / 加权总分 / 指数), how many patterns were cleared, and confirm that no fact was added or lost. A rewrite that lowers the score by deleting facts is a failed edit; so is one that raises the count of facts in the text.
### Step 4: Optional: style clone, variants, tone, review
After; **a paraphrase candidate, not a deletion-only edit; review whether “能融入” preserves the intended claim**:
- 不仅…更是 → **pattern 1** (不是 X，而是 Y; negation-half padding)
Six stylistic patterns targeted; semantic equivalence still requires review: the rewrite keeps 性能卓越 and 智能生活, the only two claims present in the source. It does **not** gain a rotation speed, a battery life or a waterproof rating; those are not in the source, and inventing them is exactly the failure mode this skill exists to prevent.
Pattern 31 (聊天机器人残留); the wrapper adds nothing; the content stands alone.
- ✅ Quote the pattern number for every change; it makes the edit reviewable and teachable.
- ✅ Respect 弱证据; two hits in one paragraph, not one hit anywhere.
- ❌ Don't invent facts to make the rewrite concrete; if the source has no number, ask for one.
- ❌ Don't "polish" a draft into formal register; that usually adds AI smell rather than removing it.
- **Keep-conditions need human confirmation.** Some removal is context-dependent; a safety caveat such as "do not soak for long periods" may be a legal requirement; when a pattern overlaps with a claim that must stay, ask before deleting.
- [`references/ai-patterns-zh.md`](references/ai-patterns-zh.md); the full 35-pattern Chinese AI-smell catalog, grouped into 摆姿势 / 机械节奏 / 注水借势 / 格式装饰 / 助手残留, each entry giving 识别特征 → 为什么假 → 改前/改后.