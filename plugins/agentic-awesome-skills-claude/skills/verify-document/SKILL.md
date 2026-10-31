Inspect a document for forensic authenticity signals; not a fraud verdict, but a risk band with the evidence behind it. Uses the Stipple API (free anonymous tier, no signup).
4. **Interpret the response.** Two independent axes; read both:
   A clean phone photo of a real payslip is commonly `low` risk + `limited` quality; **low coverage is not risk**. Per-signal `evidence` includes: amount/words mismatch, font discontinuity in values, date anomalies, document label integrity, identifier checksums (ABN/ACN/TFN), table arithmetic.
   - "risk_band: LOW; nothing looks tampered"
   - "inspection_quality: limited; couldn't inspect everything; low coverage is NOT fraud"
risk_band:           LOW; Nothing looks tampered.
- Document types the engine recognizes (payslips, invoices, bank statements) get type-specific checks; unrecognized types get generic checks only; say so in your report