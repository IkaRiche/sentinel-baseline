# 10-Minute Quickstart

> **TL;DR**: Four changes that immediately harden your agent. No commands here—just what to configure.

---

## Before You Start

This guide assumes you already have an AI agent running on a social platform. It doesn't matter which platform or framework—these principles apply universally.

**Time required**: ~10 minutes  
**Skill level**: Basic familiarity with your agent's configuration

---

## Step 1: Disable Dangerous Capabilities

### Remove Shell Access

Your agent should **never** have the ability to execute shell commands, terminal operations, or system calls.

**What to check:**
- Agent configuration for "shell", "exec", "terminal", "command" options
- Skill/plugin settings that enable system execution
- Framework features that allow code evaluation

**Target state**: Agent cannot execute arbitrary system commands under any circumstances.

---

### Disable Skills Installation

If your platform supports "skills" or "plugins", your agent should **not** have the ability to install them.

**What to check:**
- Agent permissions for skill/plugin installation
- Auto-install or auto-enable settings
- Framework features that download external code

**Target state**: Agent cannot add new capabilities without human intervention.

---

## Step 2: Restrict Network Access

### Enable Egress Allowlist

Your agent should only be able to reach the specific services it needs—nothing else.

**What to configure:**
- Firewall rules (at OS or container level)
- Allowed destinations: your API, your platform, your authority service
- Block everything else by default

**Target state**: Agent can only connect to explicitly approved endpoints.

---

## Step 3: Add Authority Layer

### Separate Decision from Execution

The core principle: your LLM should **propose** actions, not **execute** them.

**Architecture change:**
```
Before: LLM → Action
After:  LLM → Authority → Receipt → Action
```

**What to implement:**
- Separate service that evaluates policy
- Receipt/signature system for approved actions
- Executor that only accepts signed receipts

**Target state**: LLM cannot directly trigger any external action.

---

## Step 4: Set Default-Deny Policy

### Block Everything Not Explicitly Allowed

Your policy should start from "deny all" and add specific allows.

**Policy structure:**
```
ALLOW specific_action WHERE conditions
ALLOW another_action WHERE conditions
DENY * (implicit or explicit)
```

**What to check:**
- Default behavior when policy doesn't match
- Fallback handling for unknown actions
- Error states (should fail closed, not open)

**Target state**: Any action not explicitly permitted is blocked.

---

## Verification Checklist

After completing the steps above, verify:

| Check | Expected |
|-------|----------|
| Attempt shell command | Blocked/Error |
| Attempt skill install | Blocked/Error |
| Request to blocked domain | Connection refused |
| Action without receipt | Rejected |
| Authority unavailable | Agent stops (fail-closed) |

---

## What's Next?

1. **[Take the Scorecard](./SCORECARD.md)** — Verify your current state
2. **[Understand Receipt-Gating](./RECEIPT-GATING.md)** — The technical pattern
3. **[Use the templates](./templates/)** — Ready-made notices and checklists

---

## Copy/Paste Section

```
Sentinel Baseline — 10-Minute Hardening

□ Disabled shell access
□ Disabled skills installation
□ Enabled network egress allowlist
□ Added authority layer (Decision ≠ Execution)
□ Set default-deny policy
□ Verified fail-closed behavior

All checks complete? Your agent is baseline-hardened.
```

---

*Next: [RECEIPT-GATING.md](./RECEIPT-GATING.md) — Technical deep-dive*
