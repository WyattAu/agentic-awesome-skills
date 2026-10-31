- Workspace required; extract from leaked repo URL or Atlassian dork results.
- For root AWS keys, infrastructure-write GitHub PATs, or admin Slack tokens; flag for the operator and let them decide.
**AWS access key; IAM enum:**
# CloudTrail check: is logging on?
**GitHub PAT; repo enum:**
# Per-org: members, repos, secrets (secrets endpoint is metadata-only: names not values)
**Slack token; workspace enum:**
**JWT; full triage workflow:**
# Look for: kid (key ID: possible JKU/X5U injection target)
**Postman PMAK; workspace enum:**
**Anthropic API key; usage enum:**
**OpenAI API key; usage enum:**
**Generic key; provenance enum:**