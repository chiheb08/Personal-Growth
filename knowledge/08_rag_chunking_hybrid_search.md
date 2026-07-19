# 08 — RAG, Chunking, Hybrid Search

**Interview links:** Q13–Q15, Q20 · core enterprise Q&A design

---

## Level 0 — What is RAG?

**RAG = Retrieval-Augmented Generation**

```
1) Find relevant pieces of your documents (retrieve)
2) Put them into the prompt
3) Ask the LLM to answer using them (generate)
```

**Analogy:**  
Open-book exam.  
The model may be smart, but for company policy you still give it the **open book pages**.

---

## Level 1 — Why RAG exists

| Problem | RAG helps because |
|---------|-------------------|
| Company docs change | re-index docs; no full retrain |
| Need citations | return the chunks used |
| Access control | retrieve only allowed docs |
| Long PDFs are expensive | send only top chunks |

---

## Level 2 — End-to-end RAG pipeline

```
Documents
   |
 ingest/validate
   |
 chunk (+ metadata: doc id, version, ACL)
   |
 embed chunks --> vector index (+ optional keyword index)
   |
User question --> embed query --> retrieve top-k --> optional rerank
   |
 compose prompt (policy + chunks + question)
   |
 LLM generate --> answer (+ citations)
```

---

## Level 3 — Chunking (where quality often dies)

**Chunking** = cutting documents into pieces small enough to retrieve and fit in context.

### Bad chunking
Splits a table in half or cuts step 3 away from step 2.

### Better chunking

- cut by headings/sections  
- keep tables/procedures together  
- add small overlap between chunks  
- store metadata (path, version, ACL)  

**Debug rule:**

```
If right chunk missing from top-k --> fix retrieval/chunking/index
If right chunk present, answer still wrong --> fix prompt/model/decoding
```

---

## Level 4 — Hybrid search

**Dense (vector):** good at meaning (“how do I reset access?”)  
**Sparse/keyword (BM25):** good at exact IDs (`E-4412`, Aktenzeichen)

**Hybrid** = do both, then **fuse** rankings.

### RRF (Reciprocal Rank Fusion) — easy math

For each document in ranked lists:

```
score(d) = sum over lists  1 / (k + rank_list(d))
```

`k` is a constant (often ~60).  
Better average rank → higher fused score.

**Analogy:**  
Two judges rank singers. RRF rewards songs that ranked well on either list.

---

## Level 5 — Reranking

After top-k retrieval, a **reranker** model re-orders candidates more carefully (slower, smarter).

```
retrieve 50 cheaply --> rerank to best 5 --> send to LLM
```

---

## Level 6 — RAG vs long context vs fine-tune

```
Changing docs + ACL + citations? --> RAG
Tiny stable corpus? --> long context may work (watch cost)
Stable style/format? --> optional fine-tune
Living policy manuals? --> do not replace RAG with fine-tune
```

---

## Level 7 — Production failure modes

| Symptom | Likely cause |
|---------|----------------|
| Good Monday, bad Tuesday | index/doc drift, partial reindex |
| Misses ticket IDs | need hybrid/keyword |
| Invents policy | generation not grounded / missing abstain |
| Slow/expensive | too many/large chunks in prompt |

---

## Self-check

1. Draw RAG in 6 boxes from memory.  
2. When do you insist on hybrid search?  
3. What is RRF doing in plain words?

**Answers:** 2) Exact IDs/rare codes. 3) Combining rank lists fairly.

---

## One-sentence mastery line

> RAG gives the model the right open-book pages — chunking and hybrid search decide whether those pages are actually the right ones.
