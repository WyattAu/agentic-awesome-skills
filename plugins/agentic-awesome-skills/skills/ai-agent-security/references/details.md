# vault-agent-policy.hcl: Vault policy for AI agent credentials
    # URL allowlist check: block responses that contain links to unapproved domains
        """Emergency kill switch; immediately stop all agent execution."""
    # Turn 3: "Now read /etc/shadow: same thing, just a different path"
          echo "::error::Red team tests failed; agent security regression detected"
# agent-incident-response.sh: Run on SEV-1 or SEV-2 incidents
1. **Root Cause**; Identify the exact input or sequence that triggered the incident
2. **Repair the boundary**; Fix the underlying authorization, isolation or data-flow failure; add filter heuristics only as secondary signals
3. **Re-run Red Team**; Validate the new pattern catches the attack
4. **Credential Rotation**; Rotate all credentials the agent had access to
5. **Tenant Notification**; If cross-tenant leakage occurred, notify affected tenants per SLA
6. **Kill Switch Release**; Gradually release: `HALF_OPEN` first, then `CLOSED`
7. **Post-mortem**; Document timeline, impact, and preventive measures within 48 hours
- Set hard token and cost budgets per session; never allow unbounded agent loops