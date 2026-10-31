This skill transforms weak, vague, or inconsistent prompts into precision-engineered instructions that reliably produce high-quality outputs from any LLM (Claude, Gemini, GPT-4, Llama, etc.). It applies systematic prompt engineering frameworks; from zero-shot to few-shot, chain-of-thought, and structured output patterns.
- **R**; **Role**: Who is the AI in this interaction?
- **S**; **Situation**: What context does it need?
- **C**; **Constraints**: What are the rules and limits?
- **I**; **Instructions**: What exactly should it do?
- **T**; **Template**: What should the output look like?
Do not include any explanation or markdown; just the raw JSON object.
- ❌ **Don't:** Use negative-only instructions ("don't be verbose"); add positive alternatives
- ❌ **Don't:** Assume the model knows your codebase context; always include it
- ❌ **Don't:** Use the same prompt across different models without testing; they behave differently