# 13 — Reliability: Idempotency, Retries, Observability

**Interview links:** Q22, Q26, Q33–Q35, Q41–Q44

---

## Level 0 — Distributed systems meet LLMs

Agents fail like backend systems fail:

- timeouts  
- retries  
- partial success  
- duplicate side effects  

Prompting “please only refund once” is not enough.

---

## Level 1 — Idempotency (critical word)

**Idempotent** means: doing the same request again does **not** change the result beyond the first time.

**Analogy:**  
Pressing “elevator button for floor 5” ten times still goes to floor 5 once — not ten trips charging you ten times.

### Refund example

```
Attempt1: refund(key=K) --> success (caller times out, doesn't see it)
Attempt2: refund(key=K) --> returns SAME result (no second payout)
```

Without key:

```
two refunds --> double money moved
```

---

## Level 2 — Retries need budgets

```
tool fails
  --> retry 1..N (limited)
  --> still failing with same args? STOP and escalate
  --> circuit breaker if tool is unhealthy
```

**Circuit breaker analogy:**  
If the kitchen is on fire, stop sending new orders there for a while.

---

## Level 3 — Observability for agents

Log a **trajectory** (redacted):

```
request_id
  step1: tool X args' latency error
  step2: ...
  model_version, prompt_version
  tokens, cost
```

Metrics that matter:

- task success rate  
- steps per success  
- tool error rate  
- human escalation rate  
- € per successful task  

Without step logs, agents are undebuggable.

---

## Level 4 — Eval in CI for agents

Use **fake/simulated tools** in tests:

```
expect tool sequence
expect schema-valid args
expect stop behavior
expect side effects in the simulator
```

Gate prompt/tool version changes.

---

## Level 5 — Latency for multi-tool agents

Serial:

```
t1 -> t2 -> t3 -> t4 -> t5 -> big model   (slow)
```

Better:

```
small router -> parallel independent tools -> synthesize
stream status to UI
long work -> async job + notify
```

---

## Level 6 — Reject-your-own-design checklist

Ship only if yes:

- [ ] tool allowlist / least privilege  
- [ ] max steps / token budget  
- [ ] idempotent writes  
- [ ] HITL on high impact  
- [ ] redacted trajectory logs  
- [ ] evals on tool sequences  
- [ ] ACL on retrieval  
- [ ] fallback + runbook  

---

## Self-check

1. Define idempotency with a non-LLM example.  
2. Why do timeouts cause double charges?  
3. What is a trajectory?

**Answers:** 1) Elevator button / “mark paid” with same key. 2) Retry without dedupe. 3) Step-by-step record of the agent run.

---

## One-sentence mastery line

> Production agents fail like distributed systems — design retries, idempotency, and traces first, clever prompts second.
