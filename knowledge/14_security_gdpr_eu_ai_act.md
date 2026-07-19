# 14 — Security, GDPR, EU AI Act (practical engineer view)

**Interview links:** Q18, Q36–Q38 · Germany/public-sector realism

---

## Level 0 — Security is about blast radius

Chat-only risk: bad text.  
Agent risk: bad text that **triggers tools** (email, DB write, refund).

**Analogy:**  
A wrong sentence vs a wrong sentence that also presses industrial buttons.

---

## Level 1 — Agent-specific risks

| Risk | Plain meaning |
|------|----------------|
| Prompt injection | untrusted text overrides policy |
| Confused deputy | agent uses its privileges for attacker goals |
| Tool abuse | model calls dangerous tools wrongly |
| Exfiltration | secrets leave via tool args / answers |
| Supply chain | shady plugins/tools |

Controls: least privilege, allowlists, argument validation, DLP, rate limits, HITL.

---

## Level 2 — PII and logs (GDPR mindset)

**PII** = personal data (names, IDs, contact, etc. — legal definition is broader; partner with DPO).

Practical engineering defaults:

```
Log: action, timestamps, doc ids, model version, error codes
Avoid logging: raw prompts with citizen data (unless legal basis + protected store)
Retention: short by default
Access: locked down
Sensitive inference: on-prem route when required
```

**Minimize** what you store. If you don’t need it for audit, don’t keep it.

---

## Level 3 — Data residency / DPA

If using cloud LLM vendors:

- contracts / DPA  
- region choices  
- what is sent in prompts  

If policy forbids: gateway routes those requests to **on-prem** models (vLLM-style serving).

---

## Level 4 — EU AI Act (engineer-sized answer)

You are not the lawyer. You still must:

1. Clarify **intended use** with Legal/DPO  
2. Build **technical controls**: transparency, human oversight hooks, logging, limited autonomy, eval evidence  
3. Refuse silent full automation of high-impact decisions  

```
intended use --> risk discussion --> engineering controls --> evidence pack
```

Internal doc assistant ≠ automated rights-affecting decisions — but you still document and control.

---

## Level 5 — ACL in RAG

**ACL** = Access Control List (who may see which docs).

Retrieval must filter by permissions:

```
user can only retrieve chunks they are allowed to read
```

Otherwise the model becomes a data leak engine.

---

## Self-check

1. Why are agents riskier than chatbots?  
2. What should agent logs usually avoid?  
3. What is your EU AI Act one-liner as an engineer?

**Answers:** 1) Tools create real-world side effects. 2) Raw PII prompts without basis. 3) I implement controls and partner with compliance; I don’t claim solo legal sign-off.

---

## One-sentence mastery line

> Security and GDPR for AI means: least data, least privilege, human gates for impact, and proof you can explain what the system did.
