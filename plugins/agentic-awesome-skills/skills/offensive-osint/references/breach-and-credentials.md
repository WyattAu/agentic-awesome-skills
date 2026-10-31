- [Have I Been Pwned](https://haveibeenpwned.com/); breach lookup; Pwned Passwords API (k-anonymity).
- [Dehashed](https://dehashed.com/); credential search (paid).
- [IntelX](https://intelx.io/); data intelligence.
- [Cavalier (Hudson Rock)](https://cavalier.hudsonrock.com/); historical infostealer-log lookup reference; verify current access, costs and lawful purpose before use.
### 15.0.1 HudsonRock Cavalier: direct API recipe
- `total`; total stealer entries touching this domain.
- `totalStealers`; global stealer-log corpus size (context only).
- `employees`; count of `<*>@<domain>` accounts found.
- `users`; count of accounts where the domain appeared as a *visited* URL (customers/vendors).
- `third_parties`; accounts touching adjacent domains in the org.
- `data.employees_urls[]`; `{occurrence, type, url}`; internal apps where employees were logging in when stolen. **Subdomain hits here = recon gold.**
- `data.clients_urls[]`; same shape; user-facing apps (often reveals undocumented public portals).
- `data.stealer_families[]`; `{_key, _value}` → which stealer (RedLine / Lumma / StealC / Vidar / Raccoon).
- `data.dates_compromised[]`; `{_key, _value}` → temporal distribution.
**Employees vs end-users distinction:** a target-domain address is a candidate association, not proof of current employment. A customer-associated address is not proof of password reuse; useful for credential-stuffing risk awareness but not directly compromising the target's identity fabric.
1. **IntelX phonebook API**; 2-step search + poll. Largest single source for breach-era addresses.
2. **Hunter.io**; domain-search endpoint. Verify current quotas and the provider’s meaning of “verified”; provider labels are not independent employee confirmation.
3. **crt.sh**; extract X.509 SAN extensions. Many certs include admin/contact emails.
4. **DuckDuckGo SERP scrape**; HTML scrape of `"@{target-domain}"` results.
5. **Bing SERP scrape**; same query, complementary index.
6. **Wayback CDX**; historic snapshots of the target's homepage / contact / about pages often contain emails removed from the live site.