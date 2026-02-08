# Sentinel Security Scorecard

> **TL;DR**: 10 questions. 100 points. Know exactly where your agent stands—and what to fix first.

---

## Instructions

Answer each question honestly. Add up your points at the end.

**Scoring**:
- 10 points = Secure configuration
- 0 points = Insecure configuration
- No partial credit

---

## The Questions

### 1. Shell Access (10 pts)

**Does your agent have access to shell/terminal/command execution?**

| Answer | Points |
|--------|--------|
| NO shell access at all | **10** |
| Limited shell with allowlist | 0 |
| Full shell access | 0 |

---

### 2. Skills Installation (10 pts)

**Can your agent install, enable, or execute "skills" or plugins?**

| Answer | Points |
|--------|--------|
| NO skills installation capability | **10** |
| Skills require human approval | 0 |
| Automatic skill installation | 0 |

---

### 3. Network Egress (10 pts)

**Is your agent's network access restricted to necessary endpoints only?**

| Answer | Points |
|--------|--------|
| Strict egress allowlist (specific IPs/domains) | **10** |
| Broad but filtered | 0 |
| Unrestricted internet access | 0 |

---

### 4. Filesystem Access (10 pts)

**Can your agent read/write files outside its designated directories?**

| Answer | Points |
|--------|--------|
| Sandboxed to specific paths only | **10** |
| Read access to some system files | 0 |
| Full filesystem access | 0 |

---

### 5. Authority Separation (10 pts)

**Is the "decision" about what to do separated from the "execution" of that decision?**

| Answer | Points |
|--------|--------|
| YES - separate authority layer (not the LLM) decides | **10** |
| LLM decides, but rate-limited | 0 |
| LLM decides and executes directly | 0 |

---

### 6. Default Policy (10 pts)

**What happens when your policy doesn't explicitly cover an action?**

| Answer | Points |
|--------|--------|
| Blocked (default-deny) | **10** |
| Logged but allowed | 0 |
| Allowed (default-allow) | 0 |

---

### 7. Receipt Gating (10 pts)

**Does your executor verify cryptographic proof of authorization before acting?**

| Answer | Points |
|--------|--------|
| YES - signed receipt required for every action | **10** |
| Some actions require approval | 0 |
| No receipt/signature system | 0 |

---

### 8. Secret Management (10 pts)

**Where are your API keys and credentials stored?**

| Answer | Points |
|--------|--------|
| External secret manager, env injection at runtime | **10** |
| Environment variables (not in repo) | 5 |
| In config files or hardcoded | 0 |

---

### 9. Audit Logging (10 pts)

**Do you log all agent actions with tamper-evident records?**

| Answer | Points |
|--------|--------|
| YES - structured logs with action hashes | **10** |
| Basic logging (text files) | 5 |
| No logging | 0 |

---

### 10. Fail-Closed Behavior (10 pts)

**What happens if your authority/policy service is unavailable?**

| Answer | Points |
|--------|--------|
| Agent stops all actions (fail-closed) | **10** |
| Agent continues with cached policy | 0 |
| Agent continues without restrictions | 0 |

---

## Score Interpretation

### 🔴 0–40 Points: UNSAFE

Your agent can likely be weaponized with minimal effort.

**Top 5 Fixes:**
1. Remove shell/terminal access immediately
2. Disable skills installation capability
3. Implement network egress allowlist
4. Add authority separation layer
5. Enable default-deny policy

---

### 🟡 50–70 Points: BASIC SAFE

Better than most, but still exploitable under sophisticated attack.

**Top 5 Fixes:**
1. Add receipt-gating for all actions
2. Implement fail-closed behavior
3. Tighten filesystem sandboxing
4. Move secrets to dedicated manager
5. Add structured audit logging

---

### 🟢 80–100 Points: HARDENED

You've closed the major attack vectors. Maintain vigilance.

**Top 5 Maintenance Items:**
1. Regular policy review (monthly)
2. Authority key rotation (quarterly)
3. Audit log analysis
4. Egress allowlist updates
5. Incident response drills

---

## Copy/Paste Section

```
Sentinel Security Scorecard Result: [YOUR SCORE]/100

Assessment:
□ No shell access
□ No skills installation  
□ Network egress allowlisted
□ Filesystem sandboxed
□ Authority separation in place
□ Default-deny policy
□ Receipt-gated execution
□ Secrets externalized
□ Audit logging enabled
□ Fail-closed on errors

Learn more: github.com/IkaRiche/sentinel-baseline
```

---

*Next: [QUICKSTART.md](./QUICKSTART.md) — Apply the baseline in 10 minutes*
