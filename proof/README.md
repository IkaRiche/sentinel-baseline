# Example Audit Log

> **TL;DR**: What a real audit log looks like—with sensitive fields sanitized.

---

## Sample Entries

The file `audit-example.jsonl` contains four example entries demonstrating different decision paths:

### 1. ALLOW Path (Action Executed)

```json
{
  "ts": "2026-02-08T16:45:32.123Z",
  "action": "POST",
  "target": null,
  "content_hash": "a1b2c3d4e5f6...",
  "kilu_decision": "ALLOW",
  "receipt_hash": "7f8e9d0c1b2a...",
  "executed": true,
  "dry_run": false
}
```

**Interpretation**: The agent requested to POST. Policy allowed it. Receipt was issued. Action was executed.

---

### 2. DENY Path (Rate Limited)

```json
{
  "ts": "2026-02-08T16:48:15.456Z",
  "action": "REPLY",
  "target": "post_xyz123",
  "content_hash": "b2c3d4e5f6a7...",
  "kilu_decision": "DENY",
  "receipt_hash": null,
  "executed": false,
  "dry_run": false,
  "reason": "rate_limit_exceeded"
}
```

**Interpretation**: The agent requested to REPLY. Policy denied (rate limit). No receipt. No execution.

---

### 3. DENY Path (Action Not Permitted)

```json
{
  "ts": "2026-02-08T16:52:01.789Z",
  "action": "DM",
  "target": "user_abc",
  "content_hash": "c3d4e5f6a7b8...",
  "kilu_decision": "DENY",
  "receipt_hash": null,
  "executed": false,
  "dry_run": false,
  "reason": "action_not_permitted"
}
```

**Interpretation**: The agent requested to DM. Policy has no ALLOW rule for DMs. Blocked by default-deny.

---

### 4. TIMEOUT Path (Fail-Closed)

```json
{
  "ts": "2026-02-08T16:55:44.012Z",
  "action": "POST",
  "target": null,
  "content_hash": "d4e5f6a7b8c9...",
  "kilu_decision": "TIMEOUT",
  "receipt_hash": null,
  "executed": false,
  "dry_run": false,
  "reason": "authority_unavailable"
}
```

**Interpretation**: The agent requested to POST. Authority was unreachable. **Fail-closed**: no action taken.

---

## What's Sanitized

In production logs, these fields contain sensitive data:

| Field | Production | Example |
|-------|------------|---------|
| `content_hash` | Full SHA-256 | Truncated |
| `receipt_hash` | Full signature hash | Truncated |
| `target` | Actual user/post IDs | Generic placeholders |

The structure and decision flow are preserved while protecting specific identities.

---

## What to Look For in Audits

| Pattern | Concern Level | Meaning |
|---------|--------------|---------|
| Many ALLOWs in short time | ⚠️ | May be approaching rate limit |
| DENY: rate_limit_exceeded | ✅ | Rate limiting working |
| DENY: action_not_permitted | ✅ | Default-deny working |
| TIMEOUT cluster | 🔴 | Authority availability issue |
| ALLOW without receipt_hash | 🔴 | Should never happen |

---

## Copy/Paste Section

```
Audit Log Structure:

FIELDS:
• ts: ISO timestamp
• action: POST | REPLY | DM | ...
• target: destination ID (if applicable)
• content_hash: SHA-256 of content
• kilu_decision: ALLOW | DENY | TIMEOUT | ERROR
• receipt_hash: signature hash (if ALLOW)
• executed: boolean
• reason: denial reason (if DENY)

HEALTHY PATTERNS:
• DENY with reason = policy working
• TIMEOUT with executed:false = fail-closed working
• Every ALLOW has receipt_hash = receipt-gating working
```
