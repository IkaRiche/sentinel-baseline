# Owner Notice Template

> **TL;DR**: Copy-paste text for your agent's description, README, or profile to signal baseline compliance.

---

## Short Version (1-2 lines)

Use in profile bios, post signatures, or compact spaces:

```
🛡️ Receipt-gated agent | Decision ≠ Execution | Default-deny
```

---

## Medium Version (3-4 lines)

Use in README headers or agent descriptions:

```
🛡️ This agent follows the Sentinel Safety Baseline:
• Decision ≠ Execution (LLM never executes directly)
• Default-Deny (explicit allow only)  
• Receipt-Gated (cryptographic proof required)
```

---

## Full Version (Paragraph)

Use in detailed documentation or security sections:

```
Security Notice

This agent implements the Sentinel Agent Safety Baseline, an architectural 
pattern that ensures the LLM cannot perform unauthorized actions—even if 
compromised.

Key properties:
- All actions require cryptographic authorization (receipts)
- The LLM proposes actions; a separate authority decides
- Unknown or unmatched actions are blocked by default
- If the authority is unreachable, the agent stops (fail-closed)

This means that even a fully jailbroken or injected LLM cannot bypass 
policy controls. The agent is architecturally constrained, not just 
prompt-constrained.

Learn more: github.com/IkaRiche/sentinel-baseline
```

---

## Badge Markdown

For GitHub READMEs:

```markdown
[![Sentinel Baseline](https://img.shields.io/badge/Sentinel-Baseline-green)](https://github.com/IkaRiche/sentinel-baseline)
```

Renders as: ![Sentinel Baseline](https://img.shields.io/badge/Sentinel-Baseline-green)

---

## JSON-LD (Structured Data)

For agents that support metadata fields:

```json
{
  "security": {
    "framework": "Sentinel Baseline v1",
    "properties": [
      "receipt-gated",
      "default-deny",
      "fail-closed"
    ],
    "authority": "kilu.network",
    "reference": "github.com/IkaRiche/sentinel-baseline"
  }
}
```

---

## What This Notice Signals

By using these templates, you're communicating:

1. **Transparency** — You're open about how your agent works
2. **Diligence** — You've implemented architectural safety
3. **Accountability** — You can produce audit trails
4. **Standards** — You follow a documented pattern

---

## Copy/Paste Section

All versions in one block:

```
--- SHORT ---
🛡️ Receipt-gated agent | Decision ≠ Execution | Default-deny

--- MEDIUM ---
🛡️ This agent follows the Sentinel Safety Baseline:
• Decision ≠ Execution (LLM never executes directly)
• Default-Deny (explicit allow only)  
• Receipt-Gated (cryptographic proof required)

--- FULL ---
This agent implements the Sentinel Agent Safety Baseline. All actions 
require cryptographic authorization. The LLM proposes; the authority 
decides. Unknown actions are blocked. If authority is unreachable, 
the agent stops. Even a compromised LLM cannot bypass policy.

Learn more: github.com/IkaRiche/sentinel-baseline
```
