# CV Generator Skill: FlowCV / Canva Edition - Detailed Guide
Use @cv-generator; I want a FlowCV-ready output.
| 1 | LinkedIn profile URL | Fetch page; extract all visible sections. **If blocked or empty: immediately ask for a LinkedIn PDF; do not proceed on an empty extraction.** |
| 5 | Existing CV or draft | Upload or paste; improve only; never alter facts. |
2. Contact information (email, phone, location; public only)
- If a section is absent, mark it **[Not provided]**; do not skip silently.
Q8.  Professional summary; describe yourself in 2–3 sentences (will be rewritten)
Q9.  Work experience; for EACH role:
Q10. Education; for EACH qualification:
Q12. Projects; for each:
- Improve ATS keyword density where natural; do not keyword-stuff.
> "Career break; [brief honest reason: personal development / caregiving / travel / health]"
5. Never fabricate a detail; mark it **[Not provided]** until the user confirms.
### Chronological (default: mid / senior)
5.  Work History (company, title, dates; minimal bullets)
- Base strictly on data collected; no padding.
### Experience bullets: STAR-lite
- Different verb to open each bullet; never repeat within one role
- Never fabricate metrics; if the user says "we grew a lot", ask for specifics
### Banned phrases: machine-checkable blocklist
This is a hard gate; output is blocked until tense is correct:
**Step 1; Parse:**
**Step 2; Score:**
**Step 3; Integrate:**
**Step 4; Report (include at end of output):**
Not added (user lacks skill): 1; Salesforce
| Job titles | Sourced directly from user data; not inferred or upgraded |
| Company names | Sourced directly; not corrected, normalised, or embellished |
| Dates | Reproduced exactly as provided; no normalisation without noting it |
| Metrics and numbers | Only those provided by the user; never approximated or invented |
## Final output: deliver in this exact order
[3–5 sentence plain paragraph; no bullets, no Markdown]
[Certificate Name]; [Issuing Body]; [Month YYYY]
[Role]; [Organisation]; [YYYY–YYYY]
[ ] Certifications; do you hold any?
[ ] Languages; list any beyond English
[ ] Employment gap [Mon YYYY – Mon YYYY]; add a brief framing note
PASS  Truthfulness; all facts sourced from user data
PASS  No hallucination; no fabricated details
PASS  Tense correctness; past for completed, present for current
PASS  ATS structure; single column, no tables or images
PASS  Standard headings; all recognisable by parsers
PASS  No forbidden characters; no ➤ ✓ ★
PASS  Humanized; no banned phrases found
FAIL  [Check name]; [specific issue and location in CV]
1. [Exact location]; [Exact problem]; [Suggested fix]
2. [Exact location]; [Exact problem]; [Suggested fix]
- "Add a GitHub or portfolio URL; technical recruiters check it before the interview."
- "Three bullets begin with 'Responsible for'; replace with direct action verbs."
If yes, output as a **separate clearly labelled plain-text block**; not inline with the CV.
- Opens with a specific hook; not "I am writing to apply for…"