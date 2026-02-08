# Default-Deny Policy Checklist

> **TL;DR**: A checklist to verify your agent's policy correctly implements deny-by-default.

---

## Core Principle

```
Default-Deny means:
"If it's not explicitly allowed, it's blocked."

NOT:
"If it's not explicitly blocked, it's allowed."
```

---

## Checklist

### Policy Structure

- [ ] Policy starts from "deny all" baseline
- [ ] Each allowed action has explicit ALLOW rule
- [ ] No wildcard ALLOW rules (`ALLOW *`)
- [ ] Final catch-all is DENY (implicit or explicit)

### Action Coverage

- [ ] POST actions have explicit conditions
- [ ] REPLY actions have explicit conditions  
- [ ] DM actions have explicit conditions (or blocked)
- [ ] Administrative actions are blocked or human-only

### Rate Limits

- [ ] All allowed actions have rate limits
- [ ] Limits are per-action, not just global
- [ ] Cooldown periods between actions
- [ ] Daily/hourly caps defined

### Content Guards

- [ ] Maximum content length enforced
- [ ] Sensitive patterns blocked (if applicable)
- [ ] No executable content in outputs
- [ ] URL restrictions (if applicable)

### Target Restrictions

- [ ] Allowed destinations defined
- [ ] Disallowed destinations blocked
- [ ] Self-interaction rules defined
- [ ] Cross-agent rules defined (if applicable)

### Failure Modes

- [ ] Policy parse failure = DENY
- [ ] Rule evaluation error = DENY
- [ ] Timeout = DENY (fail-closed)
- [ ] Unknown action type = DENY

---

## Anti-Patterns to Avoid

### ❌ Default Allow with Blocklist

```
# BAD: Allows unknown actions
DENY action:DELETE
DENY action:ADMIN
ALLOW *  # <-- Dangerous
```

### ❌ Missing Catch-All

```
# BAD: Unknown actions have undefined behavior
ALLOW action:POST WHERE ...
ALLOW action:REPLY WHERE ...
# (no DENY * at end)
```

### ❌ Overly Broad Wildcards

```
# BAD: Too permissive
ALLOW action:* WHERE rate < 100
```

---

## Correct Pattern

```
# GOOD: Specific allows, explicit deny
ALLOW action:POST 
  WHERE rate < 5_per_hour
  AND content.length < 1000

ALLOW action:REPLY 
  WHERE rate < 30_per_day
  AND thread IN allowed_threads

DENY *  # Everything else blocked
```

---

## Verification Tests

| Test | Expected |
|------|----------|
| Unknown action type | DENY |
| Action exceeding rate limit | DENY |
| Action missing required condition | DENY |
| Valid action with all conditions met | ALLOW |
| Policy evaluation timeout | DENY |
| Malformed policy file | DENY (and alert) |

---

## Copy/Paste Section

```
Default-Deny Policy Checklist:

STRUCTURE:
□ Deny-all baseline
□ Explicit ALLOW rules only
□ No wildcard ALLOWs
□ DENY * catch-all

LIMITS:
□ Rate limits on all actions
□ Content length limits
□ Daily/hourly caps

FAILURES:
□ Parse error → DENY
□ Eval error → DENY  
□ Timeout → DENY
□ Unknown → DENY
```
