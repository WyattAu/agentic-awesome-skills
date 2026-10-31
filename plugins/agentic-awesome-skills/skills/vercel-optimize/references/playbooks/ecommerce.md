2. **Checkout: keep dynamic, but parallelize external calls.** Cart/checkout/payment routes are correctly dynamic. The win is in reducing their function duration; `Promise.all` for independent calls to Stripe + inventory + tax services. Cite `vercel-react-best-practices:async-parallel`.
- `vercel-react-best-practices:async-parallel`; parallelize Stripe/inventory/tax calls in checkout
- `vercel-react-best-practices:async-suspense-boundaries`; stream the checkout shell, fill cart drawer later
- `vercel-react-best-practices:bundle-defer-third-party`; defer analytics (GA, Mixpanel) post-hydration
- `https://nextjs.org/docs/app/api-reference/components/image`; for the catalog image fix
- `https://vercel.com/docs/bot-management`; for bot traffic on search/product routes