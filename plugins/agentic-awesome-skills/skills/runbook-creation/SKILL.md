## Runbook Template: Full Structure
### Step 1: [Action Name]
### Step 2: [Action Name]
- [ ] [Check 1; e.g., health endpoint returns 200]
- [ ] [Check 2; e.g., no errors in logs for 5 minutes]
- [ ] [Check 3; e.g., metrics return to baseline]
- **Primary:** @[team-lead]; [phone/Slack]
- **Secondary:** @[manager]; [phone/Slack]
### Step 1: Verify Replica Health
### Step 2: Stop Application Writes
### Step 3: Fence the Old Primary and Confirm Write Quiescence
### Step 4: Promote Replica
### Step 5: Update DNS
### Step 6: Restart Application
- **Primary:** @dba-lead; +1-555-0101
- **Secondary:** @platform-oncall; +1-555-0102
# health-check.sh: Check health of critical services
# collect-logs.sh: Gather logs from multiple sources for incident review
# cert-check.sh: Warn if TLS certificates expire within 30 days
    printf "  OK    %s; %d days remaining\n" "$domain" "$DAYS_LEFT"
# disk-cleanup.sh: Free disk space on a host