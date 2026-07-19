# 04 — KV Cache, Inference, Latency

**Interview links:** Q4, Q5, capacity under concurrent users, TTFT

---

## Level 0 — Generation is one token at a time

Chat models usually write like this:

```
The -> The cat -> The cat sat -> The cat sat on -> ...
```

Each new token needs attention over what came before.

---

## Level 1 — Without a cache (the slow way)

For every new token, recompute attention over **the entire past** from scratch.

**Analogy:**  
Every time you add one sentence to an email reply, you re-read the whole thread from the first “Hi” again. Wasteful.

---

## Level 2 — KV cache (the smart notebook)

During generation, the model stores past **Keys** and **Values** in a **KV cache**.

```
Token 1: compute K,V  --> save
Token 2: reuse saved K,V + compute new ones --> save
Token 3: reuse ...
```

**Benefit:** much faster decoding.  
**Cost:** GPU memory grows as context and concurrency grow.

---

## Level 3 — Why production engineers care

Under many users:

```
User A long chat cache
User B long chat cache
User C long chat cache
...
GPU RAM full  -->  queues, refusals, OOM, latency spikes
```

Often the limit is **memory for KV caches**, not “CPU is at 100%.”

**Ops levers:**

- max context length per request  
- fewer concurrent long chats  
- continuous batching engines (e.g. **vLLM**)  
- separate interactive traffic from heavy batch jobs  

---

## Level 4 — Latency words (easy)

| Term | Meaning |
|------|---------|
| **TTFT** | Time To First Token — how long until streaming starts |
| **TPOT / decode time** | time per output token after the first |
| **E2E latency** | full answer time (or until useful completion) |
| **p95** | 95% of requests faster than this number |

**Analogy:**  
TTFT = how long until the waiter brings the first bite.  
Decode = how fast the rest of the meal arrives.

Long prompts often hurt **TTFT** (lots of prefill attention work).

---

## Level 5 — Prefill vs decode

1. **Prefill:** process the prompt, build initial KV cache.  
2. **Decode:** generate new tokens using/extending the cache.

```
[ big prompt prefill ] ----TTFT----> first token
 then token token token ...
```

Architecture tip: shorter prompts (better RAG) improve prefill and memory.

---

## Level 6 — Tiny capacity math (intuition)

Memory for KV roughly grows with:

```
layers × heads × sequence_length × batch_or_users × bytes_per_number
```

You don’t memorize the formula — remember the **drivers**:

- longer chats  
- more parallel users  
- bigger models (more layers/heads)  

---

## Self-check

1. What problem does KV cache solve?  
2. What new problem does it create?  
3. Name one product fix for many concurrent chats.

**Answers:** 1) Avoid recomputing past K/V. 2) GPU memory pressure. 3) Limit context / smarter batching / isolate workloads.

---

## One-sentence mastery line

> KV cache remembers past attention ingredients so generation is fast — but that memory is often what limits how many users you can serve.
