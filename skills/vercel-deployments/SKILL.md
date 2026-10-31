# Production: real credentials
# Preview: staging/test credentials
# Development: local values
// app/api/geo/route.ts; Edge API route (Next.js App Router)
// middleware.ts; Deployment geolocation routing example (not authentication)
- Separate preview and production environment variables; never share API keys
- github-actions (`github-actions`); Automated deployment gates
- cloudflare-pages (`cloudflare-pages`); Alternative edge hosting
- ssl-tls-management (`ssl-tls-management`); Custom certificate setup