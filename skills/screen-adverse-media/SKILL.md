description: Screen a person or organisation for adverse media coverage, PEP status, and sanctions exposure; corroboration-gated, returns "review" never "guilty". Use when the user asks to screen someone before onboarding, partnership, or investment; for AML/CTF flows; or for ongoing counterparty monitoring.
   Name-based (via MCP; the REST intake is document-based):
   Document-based (REST; screens the person/org named in the uploaded document):
   - `rating`; the overall screening result ("review", "clear", etc.)
   - `hits[]`; adverse media articles: date, title, source, URL, summary
   - `pep[]`; PEP signals (empty = none found)
   - `sanctions[]`; sanctions list signals (empty = none found)
   | Hits found | "Review recommended; see articles below" (never "guilty") |
   | Nothing found | "No corroborated adverse media found; this is NOT a clean record; coverage is bounded by the source list" |
   | PEP signal | "PEP status identified; enhanced due diligence may apply" |
This screen returns "review", never "guilty"; every hit is corroboration-gated.
- "Nothing found" ≠ "clean record"; always state this in reports