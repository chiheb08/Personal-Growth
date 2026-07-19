# 12 — Multi-Agent, Orchestration, HITL

**Interview links:** Q29–Q32, Q39–Q40, Q45

---

## Level 0 — Orchestrator vs “the model is in charge”

In serious systems, a **workflow/orchestrator** owns control flow:

```
states, retries, timers, audit, who may write
```

The LLM proposes drafts, classifications, tool calls.  
The orchestrator **commits**.

**Analogy:**  
Pilot (LLM suggestions) + flight computer/checklist (orchestrator) + captain approval for risky moves (HITL).

---

## Level 1 — When multi-agent helps

Split when roles need different permissions or evals:

```
Researcher (read tools only)
Writer (draft text)
Checker (no write tools; compliance review)
Supervisor (routes work)
```

### When it’s theater
Five agents to answer one FAQ → extra latency, cost, failure modes.

**Default:** one agent + tools + workflow; split only with a contract.

---

## Level 2 — Handoffs without chaos

Bad:

```
pass giant chat transcript around and hope
```

Good:

```
shared STATE(case_id, step, payload)
handoff message = case_id + next_step
writes use idempotency keys
```

---

## Level 3 — HITL (Human-in-the-loop)

Force humans for irreversible / high-impact actions:

- money movement  
- external emails to citizens/customers  
- bulk deletes  
- production config changes  
- legal filings  

Pattern:

```
agent prepares proposal + evidence
   --> human approves/rejects
   --> tool executes
   --> audit log stores who approved
```

Low-risk reads/drafts can be automatic with sampled review.

---

## Level 4 — Planner: LLM or workflow?

| Situation | Prefer |
|-----------|--------|
| Tax/finance/regulated process | Deterministic workflow + LLM skills inside steps |
| Exploratory research assistant | More LLM planning with hard budgets |

Hybrid is the enterprise default.

```
WORKFLOW:
  extract (LLM) -> validate (rules) -> draft (LLM) -> approve (human) -> write (tool)
```

---

## Level 5 — “Digital junior employee” request

Leadership fantasy → your engineering translation:

1. Split read vs write  
2. List systems in scope  
3. Approval matrix  
4. Pilot **one** narrow process  
5. Measure time saved / error rate  
6. Expand in phases  

Do not promise full SAP+email+files autonomy in one quarter.

---

## Self-check

1. Who should own retries and audit — LLM or orchestrator?  
2. When is multi-agent justified?  
3. Give 2 HITL-required actions.

**Answers:** 1) Orchestrator. 2) Clear permission/role boundaries. 3) refunds, external citizen emails, etc.

---

## One-sentence mastery line

> Multi-agent is optional; orchestration and human gates are not — especially when writes can hurt the real world.
