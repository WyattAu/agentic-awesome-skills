# HUNT-LDAP: LDAP Injection & XPath Injection
> the **RFC 4515** filter grammar; all publicly verifiable references rather
- **LDAP auth bypass**; close the `uid` filter and append an always-true OR so the
- **Blind attribute exfil**; char-by-char extraction of an attribute value via a
- **userPassword hash exfil (non-AD only)**; on OpenLDAP/389-DS the
- **XPath injection auth bypass**; `' or '1'='1` against XML-backed auth.
## CRITICAL: Active Directory vs generic LDAP
| Password attribute | `userPassword`; may hold `{SSHA}`/`{MD5}`/`{CRYPT}` and **is readable** if ACL allows | `unicodePwd`; **write-only**, never returned by any search |
| Hash exfil via injection | **Possible** where ACLs leak `userPassword` | **Not possible**; there is no readable hash attribute over LDAP |
**Do not tell a reader that blind LDAP injection yields AD password hashes; it
store passwords) and auth bypass; not hash dumping. The hash-exfil technique
## LDAP filter grammar (RFC 4515): why injection works
`&` = AND, `|` = OR, `!` = NOT. **Filters are prefix/Polish notation**; the
### The special-character set: TEST EACH ONE
| `*`  | `\2a` | wildcard; matches any value |
| NUL  | `\00` | string terminator; truncates filter in C-backed servers |
  matter, and a `*` is NOT a wildcard. Test both; the payloads do not transfer.
### Phase 1: Confirm an LDAP backend (baseline first)
# ALWAYS capture a control response first: you compare everything to this.
the cleanest LDAP-injection tell; note it, you will need it as proof.
### Phase 2: Derive a controlled test from the actual expression
### Phase 3: XPath and evidence
### Phase 4: Direct LDAP confirmation and remediation
> AD has no readable password attribute; do not list "extract AD hashes" as a
## Validation: rule out the false positive BEFORE you report
      request with one paren removed returns a **filter-syntax error**; proving
      in to a real account context; not just a different error page.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.