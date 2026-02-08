# Incident Response Template

> **TL;DR**: What to do when you suspect your agent is compromised or behaving unexpectedly.

---

## Immediate Actions (First 5 Minutes)

### 1. Pause the Agent

Stop all agent activity immediately. This prevents further damage while you investigate.

**Options:**
- Enable quiet mode (if implemented)
- Stop the service
- Revoke API credentials

---

### 2. Preserve Evidence

Before making changes, capture:

- [ ] Current audit logs
- [ ] Recent agent outputs
- [ ] Authority decision logs
- [ ] Any error messages

---

### 3. Assess Scope

Quick questions:
- What actions were taken in the last hour?
- Any receipts issued for unusual intents?
- Any failed receipt verifications? (potential attack attempts)

---

## Investigation Checklist

### Authority Layer

- [ ] Review all ALLOW decisions in past 24 hours
- [ ] Check for unusual action patterns
- [ ] Verify policy hasn't been modified
- [ ] Confirm signing keys weren't rotated unexpectedly

### Network

- [ ] Review egress logs
- [ ] Check for connections outside allowlist
- [ ] Look for failed connection attempts (blocked attacks)

### Content

- [ ] Audit recent posts/messages for policy violations
- [ ] Check for encoded/exfiltrated data
- [ ] Review any skill installation attempts

---

## Response Actions

### If Malicious Content Was Posted

1. Delete/retract the content through platform
2. Document what was posted and when
3. Check if other agents interacted with it
4. Notify platform if needed

### If Credentials Were Exposed

1. Rotate all API keys immediately
2. Revoke old credentials
3. Audit usage of old credentials
4. Update stored credentials

### If Attack Was Blocked

1. Celebrate the baseline working
2. Document the attack vector
3. Consider additional policy rules
4. Share (anonymized) learnings

---

## Post-Incident

### Root Cause Analysis

- What vulnerability was exploited?
- How did the attacker gain access?
- Did the baseline prevent damage?
- What additional controls are needed?

### Reporting

For significant incidents, consider:
- Platform security team notification
- Community disclosure (if affects others)
- Documentation for future reference

---

## Copy/Paste Section

```
Incident Response Checklist:

IMMEDIATE (5 min):
□ Pause agent
□ Preserve logs
□ Assess scope

INVESTIGATE:
□ Review authority decisions
□ Check network logs  
□ Audit content output
□ Verify policy integrity

RESPOND:
□ Remove malicious content
□ Rotate credentials if needed
□ Document attack vector

POST-INCIDENT:
□ Root cause analysis
□ Policy improvements
□ Documentation update
```
