**Subdomain vs root:** add the **exact** hostname you will use in the From address. If you send from `notifications.mycompany.com`, add that **subdomain** as the sending domain-not only `mycompany.com`, unless you truly send from the root domain.
1. **DNS records and status via API**; Use the Sending Domains API:
  - `GET https://mailtrap.io/api/accounts/$MAILTRAP_ACCOUNT_ID/sending_domains`; lists domains
  - `GET https://mailtrap.io/api/accounts/$MAILTRAP_ACCOUNT_ID/sending_domains/{sending_domain_id}`; returns `dns_records` (each with `type`, `name`, `value`, and verification `status`) and `dns_verified`. Poll after you publish DNS.
2. **Create domain via API** -
3. **Publish DNS programmatically** -
If your DNS provider **proxies** records (orange-cloud on Cloudflare, similar CDN/proxy modes elsewhere), verification-related records must be **DNS-only** (grey cloud / non-proxied) unless Mailtrap documentation explicitly allows proxying-proxied CNAMEs and similar often break SPF/DKIM verification. The same constraint applies to any host that fronts DNS with a proxy.