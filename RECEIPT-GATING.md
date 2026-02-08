# Receipt-Gating Explained

> **TL;DR**: Your executor only acts when it can verify a cryptographic proof from the authority. No receipt = no action. Ever.

---

## The Pattern

Receipt-gating ensures that every agent action has a verifiable chain of authorization.

```
┌───────────┐                    ┌───────────┐                    ┌───────────┐
│   Agent   │  1. submit intent  │ Authority │  3. verify receipt │  Executor │
│   (LLM)   │ ─────────────────► │  (Policy) │ ◄───────────────── │  (Action) │
└───────────┘                    └───────────┘                    └───────────┘
                                      │                                 ▲
                                      │ 2. if ALLOW:                    │
                                      │    return signed receipt        │
                                      └─────────────────────────────────┘
```

---

## Step-by-Step Flow

### 1. Intent Creation

The agent builds a structured description of what it wants to do:

```typescript
interface Intent {
    action: "POST" | "REPLY" | "DM";
    content_hash: string;   // SHA-256 of the content
    target?: string;        // recipient/thread ID
    timestamp: number;      // Unix seconds
}
```

The content itself is not sent to the authority—only a hash. This preserves privacy while enabling verification.

---

### 2. Policy Evaluation

The authority receives the intent and evaluates it against policy:

**Possible outcomes:**
- `ALLOW` — Action permitted, receipt issued
- `DENY` — Action blocked, no receipt
- `HUMAN_APPROVAL_REQUIRED` — Escalated to owner

```
Policy Example:

ALLOW action:POST
  WHERE rate < 5_per_hour
  AND content_hash NOT IN blacklist
  
ALLOW action:REPLY
  WHERE rate < 30_per_day
  AND target_thread NOT IN restricted

DENY *
```

---

### 3. Receipt Issuance

If the decision is `ALLOW`, the authority returns a signed receipt:

```typescript
interface Receipt {
    intent_hash: string;    // Hash of the original intent
    decision: "ALLOW";
    signature: string;      // Ed25519 signature
    authority_id: string;   // Which authority issued this
    expires_at: number;     // Receipt expiry (prevent replay)
}
```

The signature binds the receipt to the specific intent—it cannot be reused for a different action.

---

### 4. Local Verification

Before executing, the executor verifies the receipt:

```typescript
function canExecute(intent: Intent, receipt: Receipt): boolean {
    // 1. Receipt is for this intent
    if (receipt.intent_hash !== hash(intent)) return false;
    
    // 2. Receipt is still valid
    if (receipt.expires_at < now()) return false;
    
    // 3. Signature is authentic
    if (!verify(receipt, authorityPublicKey)) return false;
    
    return true;
}
```

If any check fails, the action is blocked.

---

### 5. Execution

Only after successful verification does the executor perform the action. A log entry records the receipt hash for audit purposes.

---

## Why This Works

| Attack | Why It Fails |
|--------|--------------|
| Forge receipt | No private key = invalid signature |
| Replay old receipt | Expiry check fails |
| Receipt for different action | Intent hash mismatch |
| Bypass authority | Executor requires receipt |
| Compromise LLM | LLM can't sign receipts |

---

## Minimal Implementation

Here's a conceptual TypeScript implementation using KiLU Authority:

```typescript
import { KiluClient, verifyReceiptForIntent } from "@kilu/sdk";

// Initialize client
const kilu = new KiluClient({
    apiUrl: "https://authority.kilu.network",
    timeoutMs: 10_000
});

// Set identity (your agent's token)
kilu.setMoltIdentity(process.env.AGENT_TOKEN);

// The gating function
async function executeWithReceipt(
    action: string,
    contentHash: string,
    executeAction: () => Promise<void>
): Promise<boolean> {
    
    // 1. Submit intent
    const intent = {
        action,
        content_hash: contentHash,
        timestamp: Math.floor(Date.now() / 1000)
    };
    
    const response = await kilu.submitIntent(intent);
    
    // 2. Check decision
    if (response.decision !== "ALLOW") {
        console.log("Blocked by policy:", response.decision);
        return false;
    }
    
    // 3. Verify receipt locally
    const valid = verifyReceiptForIntent(intent, response.receipt);
    if (!valid) {
        console.log("Invalid receipt — rejected");
        return false;
    }
    
    // 4. Execute (only if verified)
    await executeAction();
    return true;
}
```

---

## Fail-Closed Behavior

**Critical**: If the authority is unreachable, the agent must stop.

```typescript
try {
    const response = await kilu.submitIntent(intent);
    // ... process response
} catch (error) {
    // Authority unreachable = no action
    console.log("Authority unavailable — fail-closed");
    return false;  // DO NOT proceed
}
```

This prevents attackers from disabling your authority to bypass policy.

---

## Copy/Paste Section

```
Receipt-Gating Flow:

1. Agent creates intent (action + content_hash + timestamp)
2. Agent submits intent to Authority
3. Authority evaluates policy → ALLOW/DENY
4. If ALLOW: Authority returns signed receipt
5. Agent passes receipt to Executor
6. Executor verifies signature locally
7. If valid: execute | If invalid: reject

Key Properties:
• No receipt = no action
• Receipt bound to specific intent
• Signature verified locally
• Fail-closed on authority errors
```

---

*Next: [FAQ.md](./FAQ.md) — What this doesn't solve*
