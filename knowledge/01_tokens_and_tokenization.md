# 01 — Tokens and Tokenization

**Interview links:** Q1, cost, context limits, German compounds, ticket IDs

---

## Level 0 — The idea (no jargon)

A large language model does **not** read letters like you do.

It reads **pieces of text** called **tokens**.

**Analogy:**  
You order pizza by **slices**, not by “the whole restaurant menu.”  
The model thinks in **slices of text** (tokens), not in “the whole PDF.”

---

## Level 1 — What is a token?

A **token** is a chunk of text the model uses as one unit.

Examples (approximate):

| Text | Possible tokens |
|------|-----------------|
| `cat` | `cat` (1 token) |
| `unbelievable` | `un` + `believ` + `able` (3-ish) |
| `Hallo` | often 1 token |
| `Steuerbescheid` | may split into several pieces |
| `E-4412` | may split oddly (`E`, `-`, `44`, `12`…) |

**Rule of thumb (English):**  
~100 tokens ≈ 75 words (rough, not exact).

---

## Level 2 — What is tokenization?

**Tokenization** = the recipe that cuts text into tokens.

```
"Reset password for Anna"
        |
        v
   TOKENIZER
        |
        v
 [Reset] [password] [for] [Anna]   <-- example pieces
```

Different models can use **different tokenizers**.  
So the same sentence can have **different token counts** on Model A vs Model B.

---

## Level 3 — Why this matters at work

### 1) Money
APIs often charge **per token** (input + output).

```
cost ≈ (tokens_in + tokens_out) × price_per_token
```

### 2) Context limit
“128k context” means ~128,000 **tokens**, not 128,000 characters or pages.

### 3) Correctness
If a ticket ID splits weirdly, search and understanding can get worse.

**Enterprise habit:**  
Always measure limits and cost in **tokens**. Log token counts.

---

## Level 4 — Tiny math (easy)

Suppose:

- input = 1,000 tokens  
- output = 300 tokens  
- price = $0.002 per 1,000 tokens (example only)

```
total_tokens = 1000 + 300 = 1300
cost = 1300 / 1000 × 0.002 = $0.0026
```

Scale that to 1,000,000 requests/month → suddenly architecture choices matter.

---

## Level 5 — Advanced notes (still plain)

1. **Subword tokenization** (BPE / SentencePiece style): common words stay whole; rare words split.
2. **Special tokens**: markers like “start”, “end”, tool-call markers — not normal language.
3. **Truncation**: if text is too long, the system cuts tokens — often from the middle or the start — and meaning can break.
4. **Multilingual**: German compounds and legal terms need testing; do not assume English token behavior.

---

## Self-check

1. Is a token always one word?  
2. Why can two models disagree on “how long” a prompt is?  
3. Why is pasting a whole PDF into the prompt expensive?

**Answers:** 1) No. 2) Different tokenizers. 3) Huge token count → cost + slow attention/memory.

---

## One-sentence mastery line

> A token is the model’s bite-sized piece of text — and almost everything expensive (money, limits, speed) is counted in bites, not pages.
