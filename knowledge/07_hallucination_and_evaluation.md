# 07 — Hallucination and Evaluation

**Interview links:** Q11, Q12, agent evals Q35

---

## Level 0 — What is a hallucination?

A **hallucination** is an answer that sounds confident and fluent but is **not supported** by the given context or reliable knowledge.

**Analogy:**  
A student who writes a beautiful essay that cites a book page that doesn’t exist.

Danger: it looks professional — so people trust it.

---

## Level 1 — Two failure buckets (architecture gold)

```
1) RETRIEVAL failure: wrong/missing docs entered the prompt
2) GENERATION failure: right docs were there, model still invented
```

Always ask: **Did we fetch the truth, or invent after fetching?**

---

## Level 2 — How to reduce hallucinations

| Lever | Plain meaning |
|-------|----------------|
| Grounded RAG | answer only from retrieved chunks |
| Citations | show which chunk supports the claim |
| Abstain | allow “I don’t know / not in docs” |
| Lower temperature | less random guessing |
| Tools/APIs | fetch live facts from systems of record |
| Guardrails | block/flag unsupported claims (helps; not perfect) |

---

## Level 3 — Evaluation from scratch

You cannot improve what you don’t measure.

### Offline eval (before release)
A **golden set**: curated questions with expected answers or scoring rubrics.

Score examples:

- correctness  
- faithfulness (does it stick to sources?)  
- refusal on out-of-scope  
- JSON/tool schema validity  
- latency / cost  

### Online eval (after release)

- thumbs down  
- support escalations  
- sampled human review  
- regression on golden set after changes  

```
change prompt/model/index
        |
   offline golden eval --fail--> BLOCK
        |
      pass --> canary/release --> watch online metrics
```

---

## Level 4 — Tiny “faithfulness” idea

For a claim in the answer, check overlap with retrieved text (human or automated).

```
claim supported by chunk?  yes/no
unsupported claims / total claims  --> hallucination rate proxy
```

Not perfect math — but better than “feels good.”

---

## Level 5 — Agent evals differ from chat evals

For agents, score the **trajectory**:

- right tools?  
- valid arguments?  
- stopped correctly?  
- side effects correct (use fake tools in tests)?  
- final answer grounded?  

Final sentence beauty score alone is not enough.

---

## Self-check

1. Fluent + wrong = ?  
2. Why separate retrieval vs generation failures?  
3. What should happen if golden critical cases fail?

**Answers:** 1) Hallucination. 2) Different fixes. 3) Block promote.

---

## One-sentence mastery line

> Hallucinations are confident lies — kill them with grounding, tools, and gated evals, not vibes.
