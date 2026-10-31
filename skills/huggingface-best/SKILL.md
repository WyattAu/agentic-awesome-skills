Read the returned list and select the datasets most relevant to the user's task; match on dataset id, tags, and description. Use your judgment; don't limit yourself to 2-3. Aim for comprehensive coverage: if 5 benchmarks clearly cover the task, use all 5.
3. If a highly-ranked model is slightly over budget, keep it with a "needs Q4" note; don't silently drop it
**If no device was mentioned:** skip all size filtering; just rank by benchmark score.
| ⭐1 | [org/name](https://huggingface.co/org/name) | 7B | 85.2% |; | Apache 2.0 | Yes (fp16) |
- Use `-` for benchmarks where the model wasn't evaluated
- **Run locally**; ask about their device if not already known, then give appropriate setup instructions
- **Run on HF Jobs**; point them to the HF Jobs guide: https://huggingface.co/docs/huggingface_hub/en/guides/jobs