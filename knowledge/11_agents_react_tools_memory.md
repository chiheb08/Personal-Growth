# 11 — Agents, ReAct, Tools, Memory

**Interview links:** Q21–Q28 · Part 2 foundation

---

## Level 0 — Chatbot vs agent

### RAG chatbot (mostly one shot)

```
question --> retrieve docs --> answer
```

### Agent

```
goal --> decide action --> call tool --> observe result
          ^                              |
          +--------- repeat until done --+
```

**Analogy:**  
Chatbot = librarian answers from books.  
Agent = assistant who can also **open tickets, query DB, send drafts** — with rules.

---

## Level 1 — The three parts of an agent

| Part | Role |
|------|------|
| **LLM** | brain / decision maker |
| **Tools** | hands (APIs, DB, search) |
| **Orchestrator policies** | rules: max steps, approvals, stop |

Without policies, demos look magic and production catches fire.

---

## Level 2 — Tool calling (function calling)

The model doesn’t magically touch SAP.  
It outputs a **structured request**:

```
create_ticket({ "title": "...", "priority": "high" })
```

Your **tool runtime** checks auth, schema, timeouts, then calls the real system.

**Design tools like public APIs:**

- clear name + JSON schema  
- read vs write separation  
- least privilege  
- concise structured returns  
- secrets injected by runtime (not pasted into prompt)  

---

## Level 3 — ReAct (Reason + Act)

Pattern:

```
Thought: I need the ticket status
Act: get_ticket("T-12")
Observe: {"status":"open"}
Thought: now I can draft a reply
Act: ...
```

Good for short hops.  
Risky alone for long regulated workflows (harder to audit; easier loops).

---

## Level 4 — Plan-and-execute

```
1) Make a plan
2) Execute steps
3) If world changed / tool failed --> replan
```

Clearer for humans/evals, but needs replanning when reality shifts.

---

## Level 5 — Why naive agents fail

| Failure | Plain cause |
|---------|-------------|
| Infinite loops | no max steps |
| Retry storms | no retry budget / circuit breaker |
| Double email/refund | no idempotency |
| Huge bills | unlimited tokens/steps |
| Undebuggable | no trajectory logs |
| Dangerous writes | tools too powerful |

---

## Level 6 — Memory

### Short-term (working memory)
Current conversation + recent tool results.  
Must be size-capped; summarize near context limit.

### Long-term memory
Preferences / past cases stored in DB or vectors with **ACL + retention**.

**Critical:**  
For compliance policies, prefer **retrieve current docs** over “what we chatted last week.”

**Analogy:**  
Sticky notes on your monitor (short-term) vs filing cabinet with locks and expiry dates (long-term).

---

## Level 7 — Stop conditions (must design)

```
done when goal met
OR max_steps reached
OR tool repeatedly fails the same way
OR user cancels
OR policy requires human
```

Always tell the user what failed.

---

## Self-check

1. What makes something an agent vs RAG chat?  
2. What is ReAct in one line?  
3. Name 3 hard limits every production agent needs.

**Answers:** 1) Multi-step tool actions. 2) Interleave thinking and tools. 3) max steps, idempotent writes, HITL on high impact.

---

## One-sentence mastery line

> An enterprise agent is a bounded loop: decide, act with typed tools, observe, stop — with memory and policy, not vibes.
