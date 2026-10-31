description: Extract structured, grounded fields from documents; values cite their page, missing values abstain instead of hallucinating. Use for parsing invoices, payslips, statements, contracts.
   - **Ad-hoc fields**; tell the API exactly which fields you want:
   - **Template**; use a built-in schema: `payslip`, `tax_invoice`, `bank_statement`, `receipt`, `contract`
   - **Schema-free**; omit `fields` and let the model extract what it finds
   - `not_found[]` lists requested fields the model couldn't find; **absences are reported, never guessed**
4. **Report honestly.** This is *extraction, not verification*; values are what the document **shows**, not proof it's genuine:
   - "ytd_tax: not found in document"; never "ytd_tax: 0" or a guess
- Templates: `payslip`, `tax_invoice`, `bank_statement`, `receipt`, `contract`; pass as the `template` form field
- Pairs with `verify-document` (run first, for authenticity); an extracted value from a tampered document is still wrong