description: "Use when building apps that collect user data. Ensures privacy protections are built in from the start-data minimization, consent, encryption."
- Use when deploying to production-verify privacy controls before launch
**GDPR (EU)**; Primary reference. Article 25 mandates "data protection by design and by default." Applies to EU users and often adopted globally.
**CCPA (California)**; Right to know, delete, opt-out of sale. Similar principles: minimize, disclose, allow control.
**LGPD (Brazil)**; Aligned with GDPR. Purpose limitation, necessity, transparency. Applies to Brazil users.
**Data minimization**; Less data = less breach impact, lower storage cost, simpler compliance. Each field is a liability.
**Purpose limitation**; Assess the lawful basis and compatibility of reuse under GDPR; consent is not universally required for every processing activity. Document purpose in schema or metadata.
**Retention**; Indefinite storage increases risk and violates GDPR. Define `retention_days` per data type; automate cleanup.
**Logging**; Logs often leak PII. Redact emails, IDs, tokens. Use structured logging with allowlists.
**Third parties**; Every SDK (analytics, crash reporting, ads) may send data elsewhere. Audit dependencies and establish the applicable consent/lawful-basis requirements before loading optional tracking.
### JavaScript/Node: Minimal User Model
### JavaScript: Consent Before Tracking
### Python: Safe Logging
### SQL: Schema with Purpose and Retention
### API: Return Only Needed Fields
- ❌ Don't assume "we'll add privacy later"-it rarely happens