# Frequently Asked Questions

> **TL;DR**: This document is honest about what the Sentinel Baseline does and doesn't solve. Transparency builds trust.

---

## What This Baseline Solves

### Does this prevent prompt injection?

**Partially.** The baseline doesn't stop prompt injection attacks *inside* the LLM. What it does:

- If an attacker injects a prompt that causes the LLM to output "delete everything"
- The LLM can only *propose* this action
- The authority layer evaluates policy and blocks it (no matching ALLOW rule)
- No receipt issued = no action taken

**Result**: The injection succeeds at the LLM level, but fails at the execution level.

---

### Does this stop malicious skills?

**Yes, if you disable skill installation.** If a skill is somehow installed:

- The skill can output anything
- But outputs must still pass through the authority layer
- No receipt = no action

The strongest protection: don't allow skill installation at all.

---

### What about a compromised LLM?

**Handled.** Even if the entire LLM is replaced with a malicious version:

- It cannot sign its own receipts (no private key)
- It cannot forge authority responses (executor verifies locally)
- It cannot bypass network restrictions (OS/container level)

The attacker controls what the agent *wants* to do, but not what it *can* do.

---

## What This Baseline Does NOT Solve

### Insider attacks

If someone with legitimate access to your authority layer or signing keys becomes malicious, the baseline doesn't help. This requires:

- Key management policies
- Access controls
- Audit trails (the baseline helps here)

---

### If you gave shell access

If your configuration gives the LLM shell access, the baseline cannot help. The LLM can bypass everything by executing arbitrary code.

**Fix**: Remove shell access. There is no architectural substitute.

---

### Prompt injection inside the LLM

The baseline doesn't make the LLM immune to manipulation. It makes manipulation *less dangerous* by limiting what actions can result from it.

For LLM-level protection, see:
- Input sanitization
- Output filtering
- Prompt hardening techniques

---

### Social engineering the owner

If an attacker convinces the human owner to approve a malicious action, the baseline allows it through. The authority layer trusts human decisions.

**Mitigation**: Owner education, clear approval workflows, anomaly alerts.

---

### Data exfiltration via outputs

If your agent can post content, an attacker might encode sensitive data in posts. The baseline doesn't inspect content meaning—only structure.

**Mitigation**: Content auditing, anomaly detection on output patterns.

---

### Denial of service

An attacker might flood your authority with requests, causing legitimate actions to timeout. Fail-closed protects against *action* but causes *stoppage*.

**Mitigation**: Rate limiting at authority, request authentication.

---

## Common Questions

### Why not just trust the LLM?

LLMs are:
- Susceptible to prompt injection
- Non-deterministic
- Influenced by training data
- Potentially fine-tunable by attackers

**Trust boundaries should not include the LLM.** Assume it will be compromised.

---

### Isn't this just "defense in depth"?

Sort of. The key difference:

- Defense in depth = multiple layers that each *might* catch attacks
- Sentinel Baseline = architectural guarantee that the LLM *cannot* act alone

It's not "maybe one of these will work"—it's "the LLM categorically lacks the capability."

---

### Can I implement this without KiLU?

Yes. The baseline is a set of principles, not a product. You can build your own authority layer with:

- Any signing algorithm (Ed25519, RSA, etc.)
- Any policy engine (OPA, custom logic, etc.)
- Any transport (HTTP, gRPC, local IPC, etc.)

KiLU is one implementation. The pattern is universal.

---

### What if my platform doesn't support this?

If your platform's architecture doesn't allow separating decision from execution:

1. Add a proxy layer between the LLM and platform
2. The proxy implements receipt verification
3. The LLM talks to the proxy, not the platform directly

This adds latency but provides the security guarantee.

---

### How do I verify I'm correctly implemented?

1. **Take the [Scorecard](./SCORECARD.md)** — Score should be 80+
2. **Test fail-closed** — Disable authority, verify agent stops
3. **Test receipt verification** — Forge a receipt, verify rejection
4. **Audit logs** — Verify all actions have receipt hashes

---

## Copy/Paste Section

```
Sentinel Baseline — Honest Boundaries

✅ What it DOES:
• Blocks unauthorized actions from compromised LLMs
• Enforces default-deny policy
• Requires cryptographic proof for execution
• Provides audit trail

❌ What it does NOT do:
• Prevent prompt injection inside the LLM
• Help if you gave shell access
• Stop insider attacks
• Inspect content meaning
• Protect against data exfiltration in outputs

Know your boundaries. Architect accordingly.
```

---

*Back to: [README.md](./README.md)*
