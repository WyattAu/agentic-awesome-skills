- Login / authentication pages; force login with attacker credentials
X-Frame-Options: DENY              # strongest; blocks all framing
**Step 1; Header check (screening).** Fetch the target page and inspect the response headers:
**Step 2; Prove it actually frames and clicks (required for a real finding).** Build a minimal PoC and load it in a real browser:
<h1>Win a prize; click below</h1>
- The page **actually renders inside the iframe** (no framebusting JS that blanks/redirects it; e.g. `if(top!==self)` breakout, or a `Sec-Fetch-Dest`/JS frame check).
- The **sensitive action still works while framed**; critically, the action must succeed *cross-site*. If it relies on a session cookie set `SameSite=Lax` or `SameSite=Strict` (the modern default), the cookie is **not** sent on the cross-site framed request, and the clickjack fails. Verify the victim's authenticated state carries into the frame.
**Strategy:** target the most sensitive action pages first; severity scales directly with what the victim is tricked into doing.
- Public, read-only pages (home/marketing) lacking frame protection are low/informational; no sensitive action to redress.
- **Header-absence alone is NOT a finding.** SameSite cookies, framebusting JS, or the lack of any sensitive framed action can each fully defeat it; which is why Step 2 is mandatory.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.