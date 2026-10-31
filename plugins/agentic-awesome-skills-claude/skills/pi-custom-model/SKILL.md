- `~/.pi/agent/settings.json`; `defaultProvider`, `defaultModel`, `defaultThinkingLevel`
- `~/.pi/agent/models.json`; custom models, keyed by provider
- `~/.pi/agent/auth.json`; provider credentials (check the provider key exists)
3. **Add the model to `models.json`** under `providers.<provider>.models`. For a **built-in provider** (openrouter, anthropic, etc.) you only supply metadata; `api`, `baseUrl`, and auth are inherited from the bundled defaults. Example:
- **Exact match only.** `find()` is exact `provider`+`id`; no fuzzy/colon-stripping for the *saved default* path. The slug in `settings.json` and `models.json` must be byte-identical.
- **Don't edit `settings.json` alone.** Setting `defaultModel` to an unregistered slug does nothing; `models.json` is the actual fix.