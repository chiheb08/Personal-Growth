# 02 — Embeddings and Similarity

**Interview links:** Q2, RAG retrieval, hybrid search, vector index

---

## Level 0 — The idea

An **embedding** turns text into a **list of numbers** (a vector) so a computer can measure “how similar in meaning.”

**Analogy:**  
Two songs can sound alike even if lyrics differ.  
An embedding is like a **music fingerprint for meaning**.

```
"reset password"  -->  [0.12, -0.44, 0.91, ...]
"change my pwd"   -->  [0.11, -0.41, 0.88, ...]   (nearby)
"pizza recipe"    -->  [-0.70, 0.20, 0.05, ...]    (far away)
```

---

## Level 1 — Vector = arrow of numbers

In 2D (toy world), a vector is just `(x, y)`.

In real models, a vector might have **384, 768, 1024…** numbers (dimensions).  
You cannot draw that, but the idea is the same: **position in meaning-space**.

---

## Level 2 — Similarity (the useful math)

### Dot product (easy intuition)
If two arrows point a similar direction, their **dot product** is larger.

For vectors `a` and `b`:

```
a · b = a1*b1 + a2*b2 + ... + an*bn
```

### Cosine similarity (most common in search)
Ignores length; cares about **angle** (direction of meaning).

```
cos(a, b) = (a · b) / (|a| * |b|)
```

- `1` ≈ same direction (very similar)  
- `0` ≈ unrelated  
- `-1` ≈ opposite (rare in text embeddings practice)

**Analogy:**  
Cosine asks: “Are we facing the same way?” not “Are we the same height?”

---

## Level 3 — Embedding model vs chat LLM

| | Embedding model | Chat LLM |
|--|-----------------|----------|
| Job | Turn text → vector | Write answers |
| Output | numbers | text / tool calls |
| Used for | search, clustering | generation |

**RAG uses both:**  
embed to **find** chunks → LLM to **write** the answer.

---

## Level 4 — Vector index (search engine for meaning)

You do not compare a query to 10 million docs one-by-one every time (too slow).

You store vectors in an **index** (FAISS, pgvector, OpenSearch kNN, etc.) that finds **approximate nearest neighbors** fast.

```
Query text --> embed --> query vector
                              |
                              v
                     VECTOR INDEX top-k
                              |
                              v
                        similar chunks
```

---

## Level 5 — Advanced but practical

1. **Same embedding model for index and query** — mixing models breaks similarity.
2. **Re-embed after model change** — old vectors are not compatible.
3. **Normalization** — many systems L2-normalize vectors so cosine ≈ dot product.
4. **Dense vs sparse:**
   - dense = embedding vectors (meaning)
   - sparse = keyword scores like BM25 (exact words)
5. Hybrid search combines both (see file 08).

---

## Mini example (toy numbers)

```
q = [1, 0]
a = [0.9, 0.1]   # similar
b = [0, 1]       # different

cos(q,a) ≈ 0.99
cos(q,b) = 0
```

So search returns `a` before `b`.

---

## Self-check

1. Can an embedding model write a paragraph? (usually no — wrong tool)  
2. Why is cosine popular?  
3. What breaks if you change embedding models but keep old vectors?

**Answers:** 1) Use chat LLM for writing. 2) Compares meaning direction. 3) Similarity becomes nonsense — re-index.

---

## One-sentence mastery line

> Embeddings put meaning on a map of numbers so “nearby” means “similar,” which is how RAG finds the right paragraphs.
