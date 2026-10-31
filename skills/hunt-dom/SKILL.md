# HUNT-DOM: DOM Clobbering / PostMessage / Service Worker / CSS Exfil
DOM-based attacks execute in the victim's browser; some sources never reach the server, while others do; assess the actual data path rather than assuming WAF/input filters are irrelevant. PostMessage missing-origin-check = cross-origin token theft with no XSS needed.
- **DOM Clobbering → DOM-XSS / auth bypass**; HTML *markup* injection (no `<script>`) overwrites a JS global like `window.config` or a form named property, and the app later treats that value as a URL/code → sink fires under a markup-only injection where script is filtered.
- **PostMessage no origin check → session theft / DOM-XSS**; a `message` handler that trusts `event.data` without validating `event.origin` lets an attacker iframe/opener drive privileged actions or feed a sink.
- **Service Worker abuse**; register a **same-origin** SW script (reachable because of an authorized same-origin JavaScript upload/path) via stored XSS → intercept all in-scope `fetch` → persistent credential capture.
- **CSS Exfil**; attribute-value selectors (`input[value^="a"]`) leak a CSRF token / API key / nonce char-by-char to an OOB host with zero JS.
### Grounding: public research this is distilled from
- **DOM Clobbering / DOM-Invader**; Gareth Heyes & the PortSwigger Web Security Academy "DOM clobbering" topic; DOM-Invader ships a dedicated clobbering scanner. Sink taxonomy maps to the academy's DOM-based vulnerability labs.
- **DOMPurify clobbering & mXSS bypasses**; Michał Bentkowski (Securitum) blog series on bypassing HTML sanitizers via clobbering and mutation XSS.
- **jQuery `htmlPrefilter` self-closing-tag XSS**; **CVE-2020-11022** and **CVE-2020-11023** (jQuery < 3.5.0). Passing attacker HTML to `.html()` / `.append()` mutates into executing markup. Grep bundled jQuery version; this is one of the most common real-world DOM-XSS roots.
- **CSS exfiltration**; d0nut "CSS Injection Attacks" / "Stealing Data With CSS" research (sequential `@import` recursion to drop the per-char-position constraint).
> Cite only what you reproduce. Do not paste these as "proof" in a report; your PoC against the live target is the evidence. Named research here is for *technique provenance*, not severity inflation.
## Phase 1: DOM Clobbering
# DOM-Invader (Burp) → enable "DOM clobbering": it auto-finds clobberable sources→sinks.
## Phase 2: PostMessage Hijacking
> False-positive guard: a handler with a *partial* check (`origin.indexOf('target.com')>-1`, `endsWith('target.com')`, regex `target\.com`) is still vulnerable; bypass with `target.com.evil.com` or `eviltarget.com`. Confirm by serving the PoC from such a look-alike host and showing the message still lands.
## Phase 3: Service Worker Abuse
**Hard rule (corrects a common mistake):** a SW script URL **must be same-origin** as the page calling `register()`. A cross-origin script URL (`https://evil.com/sw.js`) throws `SecurityError`; there is **no header that enables cross-origin SW *script* registration**. `Service-Worker-Allowed` only widens the **scope** a same-origin script may control, not where the script may live.
## Phase 4: CSS Injection / Exfiltration
> **Scope caveat (corrects an overstatement):** CSS exfil bypasses CSP that blocks *script execution*; it does **not** bypass a CSP whose `style-src` / `img-src` / `default-src` / `connect-src` restricts external origins, or `form-action`. If `img-src 'self'` is set, `url(https://OOB/...)` is **blocked**. Always read the live `Content-Security-Policy` header first; if external resource origins are locked down, CSS exfil is dead and you should say so rather than claim it.
# position's rules only after the current prefix matched (d0nut technique) -
## Phase 5: dangerouslySetInnerHTML / framework sinks
## Phase 6: Client-Side Template Injection
# DOM Invader (built into Burp browser): sources→sinks, postMessage logger, clobbering scanner
# postMessage-tracker: Chrome extension logging cross-window messages
# Burp Collaborator / interactsh / request-bin: only an approved synthetic-canary OOB sink for CSS-exfil & SW PoCs
- **DOM Clobbering**; show the clobbered value actually reaching a sink (XSS payload executes, or app navigates/loads from attacker URL). A clobberable global that never reaches a sink = no impact, do not report.
- **PostMessage**; distinguish a *missing* check from a *weak* one; bypass weak checks from a look-alike origin and capture via OOB. A noisy `message` log alone is not proof; show the privileged action or token exfil.
- **CSS exfil**; Use reproducible synthetic-canary evidence; an OOB callback is one possible proof. Read CSP first: `img-src`/`style-src`/`connect-src`/`default-src` restricting external origins kills it. A blocked `url()` is indistinguishable from success in the Network tab; confirm on the Collaborator side.
- **Service Worker**; registration must be **same-origin script**; a `SecurityError` means you cited the wrong origin. Prove *persistence* (close tabs → reopen → fresh OOB hit, no XSS re-fire).
- **General**; unique per-test markers (`btoa(domain)+nonce`) so an OOB hit is attributable to YOUR payload and not background traffic; body-diff the rendered DOM, not the raw HTML, since these are client-side.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.