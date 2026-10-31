description: Estimate whether a document's prose was written by AI, with the linguistic tells and honest abstention on non-prose. Use when the user asks whether an essay, report, CV, submission, or article was AI-generated; for triage, not proof.
   - `applicable: false`; the document is not prose (forms, tables, scans, spreadsheets). Detection is **deliberately refused** rather than guessed. Report this and stop.
   - `probability`; model confidence (0–1), NOT a calibrated truth
   - `lean`; "ai" | "human" | "unsure"
   - `tells[]`; the specific phrases/patterns flagged (e.g. "It is important to note that", uniform sentence length, low burstiness)
   - `reasoning`; why the model reached its verdict
   - `limitations`; always present; the API states its own noise profile
- Non-prose documents (forms, tables, payslips) get `applicable: false`; the engine refuses rather than guessing
- Can false-flag templated/coached or non-native-English writing; always present as one signal alongside human review