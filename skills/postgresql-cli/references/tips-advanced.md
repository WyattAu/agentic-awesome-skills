# psql Tips: Advanced Debugging, Performance & Safety
-- The '' is an escaped quote, so the entire value is a single string literal; NOT injected.
# But you can't mix freely: URI overrides individual flags
- `\i filename`; resolves relative to the **current working directory** (where psql was started)
- `\ir filename`; resolves relative to the **currently executing script's directory**