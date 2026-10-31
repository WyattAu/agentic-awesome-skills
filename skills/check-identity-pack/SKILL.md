Run an AFP 100-point or AUSTRAC safe-harbour identity check over a document set. Reports the points attained, per-document status, and **exactly what's missing**; so the user can request only the absent documents and re-run. Uses an external Stipple API; verify current access, costs and data-processing terms before uploading.
   - `afp_100_point`; the provider’s AFP checklist mode; it is not a universal Australian onboarding rule
   - `austrac_safe_harbour`; a provider mode name, not proof of current AUSTRAC due-diligence compliance
   - `status`; complete / incomplete / failed
   - `points_total`; points attained (e.g. 95/100)
   - `checks[]`; per-document: type detected, point value, status
   - `missing[]`; **exactly what is missing** (e.g. "evidence of current residential address within last 3 months")
5. **Report with the gap list front and centre.** The product here is the *exactly-what's-missing* list; the user's onboarding UX can loop on it: request only what's absent, re-run, done.
6. **Important caveat.** Document consistency checking proposes an identity association; it does NOT answer *is this document genuine*. A forged passport that matches the name scores points. For genuineness, pair with the `verify-document` skill on each document first.
  - passport: 70 pts; ok
  - medicare_card: 25 pts; ok
- The check maps documents to the scheme and reports gaps; it does not verify document authenticity (use `verify-document` for that)