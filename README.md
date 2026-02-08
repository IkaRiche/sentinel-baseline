# Sentinel Agent Safety Baseline

> **TL;DR**: Three architectural rules that prevent AI agents from being weaponized—even if compromised. No commands here. Just principles.

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

---

## What Is This?

The Sentinel Baseline is a minimal set of **architectural constraints** for AI agents operating on social platforms. It doesn't require trust in the LLM itself—it works by limiting what any agent *can do*, regardless of what it *wants to do*.

**This repository contains:**
- [BASELINE.md](./BASELINE.md) — The 3 core rules
- [SCORECARD.md](./SCORECARD.md) — 10-question self-assessment (0–100 points)
- [QUICKSTART.md](./QUICKSTART.md) — How to apply in 10 minutes
- [RECEIPT-GATING.md](./RECEIPT-GATING.md) — Intent → Receipt → Execute pattern
- [FAQ.md](./FAQ.md) — What this does and doesn't solve
- [templates/](./templates/) — Ready-to-use notices and checklists

---

## The 3 Rules

| # | Rule | What It Means |
|---|------|---------------|
| 1 | **Decision ≠ Execution** | The LLM proposes. A separate authority decides. The executor only acts on signed decisions. |
| 2 | **Default Deny** | If the policy didn't explicitly allow it, it's blocked. No exceptions. |
| 3 | **Receipt-Gated Execution** | No cryptographic proof of authorization = no action. Period. |

---

## Quick Links

- 📊 **[Take the Scorecard](./SCORECARD.md)** — Rate your agent's security in 2 minutes
- ⚡ **[10-Minute Quickstart](./QUICKSTART.md)** — Apply the baseline today
- 🔐 **[Receipt-Gating Explained](./RECEIPT-GATING.md)** — The core pattern

---

## Who Is This For?

- **Agent developers** building on social platforms
- **Platform operators** defining agent policies  
- **Security researchers** auditing agent architectures
- **Bot owners** who want to demonstrate safety

---

## Copy/Paste Section

Use this in your agent's description or README:

```
🛡️ This agent follows the Sentinel Safety Baseline:
• Decision ≠ Execution (LLM never executes directly)
• Default-Deny (explicit allow only)
• Receipt-Gated (cryptographic proof required)

Learn more: github.com/IkaRiche/sentinel-baseline
```

---

## License

CC BY 4.0 — Use freely, attribution appreciated.
