limit_memory_soft = 1610612736   ; 1.5 GB; soft kill threshold
limit_memory_hard = 2147483648   ; 2.0 GB; hard kill threshold
# Restart Odoo only (not DB: avoids data risk)
- ✅ **Do:** Store all secrets in a `.env` file and reference them with `${VAR}`; never hardcode passwords in `docker-compose.yml`.
- ✅ **Do:** Put Nginx in front of Odoo for SSL termination (Let's Encrypt / Certbot); never expose Odoo directly on port 80/443.
- ❌ **Don't:** Expose port 5432 (PostgreSQL) to the public internet; keep it on the internal Docker network only.
- ❌ **Don't:** Use the `latest` or `17` Docker image tags in production; pin a reviewed image digest; `odoo:17.0` is a mutable series tag, not an immutable patch identity.
- ❌ **Don't:** Mount `odoo.conf` and rely on it for secrets in CI/CD; use Docker secrets or environment variables instead.
- This skill covers **self-hosted Docker deployments**; Odoo.sh (cloud-managed hosting) has a completely different deployment model.
- Does not include an Nginx configuration template; consult the [official Odoo Nginx docs](https://www.odoo.com/documentation/17.0/administration/install/deploy.html) for the full reverse proxy config.
- The `addons_path` inside the Docker image may change with new base image versions; always verify after upgrading the Odoo image.