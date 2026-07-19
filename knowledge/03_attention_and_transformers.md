# 03 — Attention and Transformers

**Interview links:** Q3, Q4, Q6 · the core “how LLMs look back” topic

---

## Level 0 — The only idea you must feel

**Attention = look back and decide what matters for the next word.**

**Analogy (highlighter):**  
When you write the next word, you highlight earlier words:
- bright yellow = important  
- pale = ignore  

Then you write using the yellow parts.

**Example:**

```
The dog chased the cat because it was scared.
                              ^^
When handling "it", attention should light up "dog" and/or "cat"
(not the word "The" equally).
```

---

## Level 1 — Step by step (no math yet)

1. The model is at a position (a token).  
2. It asks: “Which past tokens help me now?”  
3. It gives each past token a **score** (importance).  
4. It **mixes** information from past tokens using those scores.  
5. That mixed info helps predict / represent the next thing.

```
past tokens --> score each --> weighted mix --> better understanding
```

---

## Level 2 — Query, Key, Value (library analogy)

This is the famous **Q, K, V**.

**Analogy: you in a library**

| Name | Plain meaning | Library |
|------|----------------|---------|
| **Query (Q)** | What am I looking for right now? | Your question card: “Where is info about *it*?” |
| **Key (K)** | How each book advertises itself | Shelf labels |
| **Value (V)** | The actual content you take if it matches | The book text |

Flow:

```
My Query  compared to  every Key  -->  match scores
scores decide how much of each Value I mix in
```

**Strong match** → take more from that word’s Value.  
**Weak match** → almost skip it.

---

## Level 3 — Tiny math (made gentle)

Each token is turned into vectors Q, K, V (by learned matrices — think “linear recipes”).

For one query vector `q` and keys `k1...kn`:

### Step A — raw scores (dot products)

```
score_i = q · k_i
```

Higher score ⇒ that past token seems more relevant.

### Step B — Softmax (turn scores into percentages that sum to 1)

Softmax is “make positive weights that add to 100%.”

```
weight_i = exp(score_i) / sum_j exp(score_j)
```

**Analogy:**  
Exam points → convert to “share of attention budget.”

Example:

```
scores = [2, 1, 0.1]
weights ≈ [0.66, 0.24, 0.10]   (sum = 1)
```

### Step C — Mix values

```
output = weight_1*V1 + weight_2*V2 + ... + weight_n*Vn
```

That output is “what I understood after looking around.”

### Optional scaling
People often divide scores by `sqrt(d)` (d = key size) so numbers don’t explode before softmax.  
You can remember: **“scale so softmax stays well-behaved.”**

---

## Level 4 — Self-attention vs “attending to a document”

**Self-attention:** tokens in the same sequence look at each other.  
That’s the default inside a transformer block.

In chat:

```
[system + docs + user + so-far answer] all sit in one sequence
attention decides which parts matter for the next token
```

---

## Level 5 — Multi-head attention (still easy)

**One head** = one highlighter style.  
**Multi-head** = several highlighters in parallel:

- one watches names  
- one watches cause/effect (“because”)  
- one watches numbers/IDs  

Then the model **combines** their notes.

**Analogy:** a small team reading the same email, each with a different job, then merging.

---

## Level 6 — Why long prompts hurt (interview gold)

In classic attention, each token can look at many others.

Rough cost idea:

```
attention work ~ sequence_length × sequence_length
               = n²
```

If `n` doubles, work can grow about **4×** (not exactly always in optimized systems, but the intuition stands).

So:

```
whole PDF in prompt  =>  huge n  =>  slow + expensive + heavy memory
RAG: only useful paragraphs => smaller n => healthier system
```

---

## Level 7 — Transformer = stacked blocks

A **transformer** (decoder-style chat model) is many layers of:

```
tokens
  --> attention (look around)
  --> feed-forward network (think locally)
  --> next layer
  ...
  --> predict next token probabilities
```

### Decoder-only (most chat LLMs)
Reads left-to-right (causal mask: cannot peek at future tokens while training/generating).

### Encoder-decoder
Encoder reads full input; decoder writes output (classic translation style).

Enterprise chat assistants: usually **decoder-only**.

---

## Whiteboard script (30 seconds)

1. Draw the sentence with “it”.  
2. Draw arrows from “it” to “dog”/“cat” with thick/thin lines (scores).  
3. Say: Q looks up Keys → softmax weights → mix Values.  
4. Say: longer text ⇒ more arrows ⇒ more cost ⇒ that’s why RAG exists.

---

## Self-check

1. In one sentence, what is attention?  
2. What do Q, K, V mean without math?  
3. Why does RAG help attention cost?

**Answers:**  
1) Look back and weight what matters.  
2) Question / label / content.  
3) Smaller sequence ⇒ less look-up work and memory.

---

## One-sentence mastery line

> Attention is a weighted look-back: score past tokens, turn scores into percentages, mix their information to write the next token.
