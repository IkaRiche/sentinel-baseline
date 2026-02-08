# Sentinel Agent Safety Baseline

> **TL;DR**: Three rules. One goal: ensure an AI agent can't be weaponized—even if the LLM is compromised.

---

## The Problem

AI agents on social platforms can:
- Post content autonomously
- Send messages to thousands of users
- Execute skills and commands

If an attacker compromises the LLM (via prompt injection, jailbreak, or malicious skill), they inherit **all these capabilities**.

The Sentinel Baseline closes this gap architecturally.

---

## Rule 1: Decision ≠ Execution

**Principle**: The component that *decides* what to do must be separate from the component that *does* it.

```
┌─────────────┐     proposes     ┌──────────────┐     decides     ┌──────────────┐
│   LLM       │ ───────────────► │   Authority  │ ───────────────►│   Executor   │
│  (Agent)    │                  │   (Policy)   │    + receipt    │   (Action)   │
└─────────────┘                  └──────────────┘                 └──────────────┘
```

**Why It Works**: Even if the LLM is fully compromised, it can only *propose* actions. The authority layer (with its own policy logic) must approve. The executor only accepts signed receipts—not LLM output.

---

## Rule 2: Default Deny

**Principle**: If the policy doesn't explicitly allow an action, it's blocked.

```
Policy Example (pseudocode):

ALLOW action:POST 
  WHERE content.length < 1000
  AND rate < 5_per_hour
  AND NOT contains(content, ["install", "execute", "run"])

DENY * // Everything else
```

**Why It Works**: You don't have to anticipate every attack vector. Anything unexpected is blocked by default.

---

## Rule 3: Receipt-Gated Execution

**Principle**: The executor only acts when it can cryptographically verify that the authority approved.

```
Flow:
1. Agent builds intent: { action: "POST", content_hash: "abc123..." }
2. Agent submits to Authority
3. Authority evaluates policy
4. If ALLOW: Authority returns signed receipt
5. Agent passes receipt to Executor
6. Executor verifies signature locally
7. If valid: execute action
8. If invalid/missing: REJECT
```

**Why It Works**: Even if the agent forges an "ALLOW" response, the executor rejects it—receipts are cryptographically bound to the authority's key.

---

## What This Achieves

| Threat | Mitigated? | How |
|--------|------------|-----|
| Prompt injection | ✅ | Agent can inject, but can't bypass policy |
| Malicious skill | ✅ | Skill can output anything, but no receipt = no action |
| Compromised LLM | ✅ | LLM can't sign its own receipts |
| Stolen API key | ⚠️ Partial | Still rate-limited, still policy-gated |
| Insider attack | ❌ | Out of scope (see FAQ) |

---

## Copy/Paste Section

```
Sentinel Agent Safety Baseline — 3 Rules

1. Decision ≠ Execution
   → LLM proposes, Authority decides, Executor verifies receipt

2. Default Deny  
   → If policy didn't allow it, it's blocked

3. Receipt-Gated Execution
   → No cryptographic proof = no action

This architecture ensures that even a fully compromised LLM 
cannot perform unauthorized actions.
```

---

*Next: [SCORECARD.md](./SCORECARD.md) — Rate your agent in 2 minutes*
