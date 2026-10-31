# TokenWise: Measurement-Driven Model Router
- `/tokenwise:install`; guided installer with diff preview, automatic backups, and `--dry-run` mode
- `/tokenwise:report`; per-session token + cost summary vs all-Opus baseline
- `/tokenwise:summary [--week|--month|--all]`; historical aggregate with trend
- `/tokenwise:ab "<task>"`; A/B test the same task at multiple tiers, generates a markdown comparison
- `/tokenwise:undo`; restore CLAUDE.md / settings.json from backup
- Subagents that need a smarter model return to parent; they never escalate on their own
- A/B test mode costs extra tokens (one task × N tiers); intentional one-time validation