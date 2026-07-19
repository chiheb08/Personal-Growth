# 05 — Training, SFT, RLHF/DPO, LoRA, Forgetting

**Interview links:** Q7, Q8 · “should we fine-tune everything?”

---

## Level 0 — Three different jobs

| Stage | Plain job |
|-------|-----------|
| **Pretraining** | Learn general language from huge text |
| **SFT** | Learn to follow instructions / formats |
| **RLHF / DPO** | Prefer better / safer answers |

**Analogy (cooking):**

- Pretrain = learn what food is  
- SFT = learn recipes customers order  
- RLHF/DPO = learn which plates get good reviews / avoid disasters  

---

## Level 1 — Pretraining (you almost never redo this)

The model predicts the next token on massive data.

```
"The capital of France is ___"  --> learns patterns of language & world text
```

Enterprise teams usually **buy/use** a pretrained base, not train from scratch.

---

## Level 2 — SFT (Supervised Fine-Tuning)

Show examples:

```
Instruction: "Extract the invoice id as JSON"
Output: {"invoice_id":"4412"}
```

The model shifts toward your **format and behavior**.

**Important:** SFT is **not** a document database.  
If policies change monthly, use **RAG**, not endless fine-tunes.

---

## Level 3 — RLHF and DPO (alignment)

### RLHF (Reinforcement Learning from Human Feedback)
Humans rank answers → train a preference signal → nudge the model.

### DPO (Direct Preference Optimization)
A more direct way to learn from “answer A is better than B” pairs.

**Plain goal:** be helpful, refuse dangerous stuff, match preferred tone — not memorize your PDF archive.

---

## Level 4 — LoRA / adapters (efficient fine-tune)

Full fine-tune changes many weights (heavy).

**LoRA** adds small trainable “side panels” (low-rank adapters) instead of rewriting the whole model.

**Analogy:**  
Don’t rebuild the entire car engine to change driving style — add a small tunable module.

Benefits: cheaper, easier to swap/rollback.

---

## Level 5 — Catastrophic forgetting

After narrow fine-tuning, the model may get **worse** at old skills.

```
Before: tools/JSON work, German OK, domain weak
After bad SFT: domain phrases OK, JSON tools broken
```

**Mitigations:**

- mix general examples into training  
- broad eval suite before promote  
- prefer LoRA + keep old artifact for rollback  
- never ship on one happy demo  

---

## Level 6 — Enterprise decision tree

```
Need current internal facts?     --> RAG
Need stable style/schema?       --> consider SFT/LoRA
Need safer tone/refusals?       --> alignment methods / policy layer
Docs change every week?         --> do NOT rely on fine-tune alone
```

---

## Self-check

1. Does fine-tuning replace RAG for changing manuals?  
2. What is catastrophic forgetting in one line?  
3. Why do teams like LoRA?

**Answers:** 1) No. 2) New training hurts old skills. 3) Cheaper/smaller changes + easier rollback.

---

## One-sentence mastery line

> Pretrain builds the brain, SFT teaches manners/format, preference methods teach taste — and RAG still feeds fresh company facts.
