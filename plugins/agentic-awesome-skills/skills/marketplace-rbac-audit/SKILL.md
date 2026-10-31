1. **Role**; what this actor type may generally do.
2. **Relationship**; which specific object the actor may access.
3. **Tenant or operational scope**; where the permission applies.
4. **Resource state**; whether the operation is valid now.
5. **Field scope**; which attributes may be viewed or changed.
- **@api-security-best-practices**; use for broader API authentication, validation, abuse controls, and secure implementation patterns.
- **@saas-multi-tenant**; use for designing tenant isolation and PostgreSQL row-level security.
- **@idor-testing**; use only for explicitly authorized offensive IDOR testing.