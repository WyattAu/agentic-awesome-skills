**Contacts** are the marketing database: lists, segments, custom fields, and imports for **campaign audiences** and related workflows. The **Contacts API** automates create/update and can feed **CRM or CDP sync** (your code, or tools like Zapier, Make, n8n; see [Import contacts](https://docs.mailtrap.io/email-marketing/contacts/import-contacts.md)).
- Rate limit (typical): **200 requests per 60 seconds** per account; prefer bulk import for large loads.
- **Lists**; explicitly defined list of contacts.
- **Segments**; dynamic groups; see [Segments](https://docs.mailtrap.io/email-marketing/contacts/segments.md).
- **Custom fields**; properties like first and last name or membership level; see [Custom fields](https://docs.mailtrap.io/email-marketing/contacts/custom-fields.md).
- **Custom events**; `POST .../events` with an event `name` and `params` object for [automations](https://docs.mailtrap.io/email-marketing/automations.md).