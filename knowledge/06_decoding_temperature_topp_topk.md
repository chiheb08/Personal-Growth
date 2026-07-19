# 06 — Decoding: Temperature, Top-p, Top-k

**Interview links:** Q9, Q10 · creativity vs reliability

---

## Level 0 — The model outputs scores, not “the answer”

For the next token, the model gives a score for many possible tokens (logits).

Then a **decoding strategy** turns scores into **one chosen token**.

---

## Level 1 — Greedy decoding

Always pick the highest score.

```
stable, less surprising, good for facts/JSON
can be repetitive in creative tasks
```

---

## Level 2 — Temperature (the creativity knob)

Temperature reshapes the score distribution before sampling.

**Easy intuition:**

- **Low temp (e.g. 0–0.3):** almost always the top choice → safer for enterprise Q&A and tool calls  
- **High temp (e.g. 1.0+):** flatter chances → more random / “creative” → more hallucinations and broken JSON  

**Analogy:**  
Low temp = always order your usual meal.  
High temp = spin a wheel of menu items.

### Tiny math sketch

If raw score for token i is `z_i`:

```
p_i = softmax(z_i / temperature)
```

- temp → small: differences get sharper  
- temp → large: differences flatten  

---

## Level 3 — Top-k

Keep only the **k** best tokens, ignore the rest, then sample.

Example `k=5`: only 5 candidates allowed.

---

## Level 4 — Top-p (nucleus sampling)

Keep the **smallest set of top tokens whose probabilities sum to ≥ p**.

Example `p=0.9`: take enough top tokens to cover 90% probability mass.

**Analogy:**  
Don’t invite the whole city to the party — invite the smallest group that already represents most of the “likely” crowd.

---

## Level 5 — Enterprise defaults (practical)

| Use case | Typical decoding idea |
|----------|------------------------|
| Factual RAG answer | low temperature |
| Tool call / JSON | low temp + schema validation |
| Brainstorming ideas | higher temp OK if labeled as draft |
| Audits / reproducibility | pin params; know “temp 0” is *more* deterministic, not always bit-identical everywhere |

---

## Level 6 — “Same prompt, different answers”

Checklist:

```
temperature / top-p different?
prompt or tool schema version different?
RAG retrieved different chunks?
different model replica/version?
cache hit vs miss?
```

Pin versions and log retrieval IDs.

---

## Self-check

1. Why is temp 1.2 dangerous for invoice extraction?  
2. What does top-p control?  
3. Is temperature 0 always identical across machines?

**Answers:** 1) More random tokens → wrong fields/JSON. 2) How much probability mass to keep. 3) Not always guaranteed bit-identical.

---

## One-sentence mastery line

> Decoding knobs decide how boldly the model gambles on the next token — enterprise truth wants small gambles.
