# Glossary — every technical word from the interview (easy definitions)

Use this while reading the interview PDF.  
Each entry: **plain meaning** · **tiny analogy** · **where it shows up**

Jump: [A](#a) [B](#b) [C](#c) [D](#d) [E](#e) [F](#f) [G](#g) [H](#h) [I](#i) [J](#j) [K](#k) [L](#l) [M](#m) [O](#o) [P](#p) [Q](#q) [R](#r) [S](#s) [T](#t) [V](#v) [W](#w)

---

## A

**ACL (Access Control List)**  
Who is allowed to see/do what. In RAG: only retrieve docs the user may read.  
*Analogy:* keycard doors per room.  
*See:* file 08, 14

**Agent (AI agent)**  
System that can loop: decide → call tools → observe → repeat until done/stop.  
*Analogy:* assistant with hands, not only a mouth.  
*See:* file 11

**Alignment (RLHF/DPO)**  
Steering the model toward preferred/safer behavior.  
*See:* file 05

**Attention**  
Look back at past tokens and weight what matters for the next step.  
*Analogy:* highlighter on important words.  
*See:* file 03

**Autoregressive**  
Generate one token after another, each depending on previous tokens.

---

## B

**BM25**  
Classic keyword search scoring (sparse retrieval). Great for exact IDs.  
*Analogy:* find pages that contain the exact code you typed.  
*See:* file 08

**BPM / workflow engine**  
Software that runs business steps with states, retries, audit.  
*See:* file 12

---

## C

**Canary release**  
Ship to a small % of traffic first; watch metrics; then expand/rollback.

**Catastrophic forgetting**  
Fine-tuning on new stuff makes the model worse at old skills.  
*See:* file 05

**Chunk / chunking**  
Cutting documents into retrieval-sized pieces.  
*See:* file 08

**Circuit breaker**  
Stop calling an unhealthy dependency for a while.  
*Analogy:* close the kitchen when it’s on fire.  
*See:* file 13

**Citation**  
Pointer to the source chunk supporting an answer.

**Context window**  
Max tokens the model can consider at once (prompt + generation limits vary by system).

**Cosine similarity**  
Measures angle between vectors (meaning similarity).  
*See:* file 02

**Causal mask (decoder-only)**  
During training/generation, a token cannot peek at future tokens.

---

## D

**Decoder-only model**  
Standard modern chat LLM style: predicts next token from left context.  
*See:* file 03

**Decoding**  
How we turn model scores into chosen next tokens (greedy, sampling…).  
*See:* file 06

**Dense retrieval**  
Search using embedding vectors (meaning search).  
*See:* file 02, 08

**DPO**  
Direct Preference Optimization — learn from “A better than B” pairs.  
*See:* file 05

**DPA**  
Data Processing Agreement — contract for processing personal data with vendors.

---

## E

**Embedding**  
Text → vector of numbers capturing meaning.  
*See:* file 02

**Encoder-decoder**  
Architecture with separate understand-input and write-output parts (classic translation style).  
*See:* file 03

**E2E latency**  
End-to-end time for a request.

**Eval / evaluation / golden set**  
Tests that score quality before/after release.  
*See:* file 07

**EU AI Act**  
EU regulation framework for AI systems; engineers implement controls + partner with legal.  
*See:* file 14

---

## F

**Faithfulness**  
Answer sticks to provided sources (doesn’t invent beyond them).

**Fallback model**  
Backup model when primary fails (429, outage, policy route).

**Fine-tuning / SFT**  
Further training on examples to change behavior/format.  
*See:* file 05

**Function calling / tool calling**  
Model emits structured calls to approved tools/APIs.  
*See:* file 11

---

## G

**Gateway (LLM gateway)**  
Central layer for keys, routing, limits, fallbacks, cost logs.  
*See:* file 10

**GDPR**  
EU data protection law — minimize personal data, lawful basis, security, retention.  
*See:* file 14

**Guardrails**  
Checks that block/flag bad inputs or outputs (policy, PII, toxicity, schema…).

---

## H

**Hallucination**  
Fluent but unsupported/wrong content.  
*See:* file 07

**Hybrid search**  
Combine vector search + keyword search, then fuse ranks.  
*See:* file 08

**HITL (Human-in-the-loop)**  
Human must approve high-impact actions.  
*See:* file 12

---

## I

**Idempotency / idempotency key**  
Retries don’t duplicate side effects; same key → same result.  
*See:* file 13

**Index (vector index)**  
Fast structure for nearest-neighbor search over embeddings.  
*See:* file 02

**Intent router**  
Classifies what the user wants (FAQ vs create ticket vs other).

---

## J

**JSON schema / structured output**  
Strict format so programs can parse model output reliably.  
*See:* file 09

---

## K

**Key (attention)**  
“Shelf label” vector used to match against a Query.  
*See:* file 03

**KV cache**  
Saved Keys/Values during generation for speed; uses GPU memory.  
*See:* file 04

**Kubernetes / K8s / OpenShift**  
Container platforms for deploying services (OpenShift is Red Hat’s K8s platform common in enterprise/gov).

---

## L

**LiteLLM**  
Common style of proxy/SDK to route to many LLM providers uniformly.  
*See:* file 10

**LLM**  
Large Language Model — model trained to predict/generate language tokens.

**LoRA**  
Efficient fine-tuning via small adapter layers.  
*See:* file 05

**Logits**  
Raw scores over next-token vocabulary before softmax.

---

## M

**Metadata (RAG)**  
Extra fields on chunks: doc id, version, section, ACL…

**Multi-agent**  
Multiple specialized agents coordinating; useful only with clear contracts.  
*See:* file 12

**Multi-head attention**  
Several attention “specialists” in parallel.  
*See:* file 03

---

## O

**Observability**  
Metrics, logs, traces so you can see and debug behavior.  
*See:* file 13

**On-prem model**  
Model served inside your environment (e.g. vLLM on your GPUs) for residency/control.

**Orchestrator**  
Component that owns workflow control, retries, budgets, tool execution.  
*See:* file 12

**OOM**  
Out Of Memory — process/GPU ran out of RAM.

---

## P

**p95**  
95th percentile latency — 95% of requests are faster than this.

**PII**  
Personal data that can identify people; handle carefully under GDPR.  
*See:* file 14

**Plan-and-execute**  
Agent pattern: plan first, then run steps (replan on change).  
*See:* file 11

**Prefill**  
Processing the prompt before the first generated token.  
*See:* file 04

**Pretraining**  
Initial large-scale training that builds general capabilities.  
*See:* file 05

**Prompt**  
The assembled input messages/instructions/context sent to the model.

**Prompt injection**  
Attack where untrusted text tries to override system rules.  
*See:* file 09

---

## Q

**Query (attention)**  
“What am I looking for?” vector at the current position.  
*See:* file 03

**Queue (async workers)**  
Holds background jobs (indexing, long tasks) so chat stays responsive.

---

## R

**RAG**  
Retrieval-Augmented Generation — retrieve then generate.  
*See:* file 08

**Rate limit / 429**  
Server says “too many requests”; need backoff/fallback.

**ReAct**  
Reason + Act loop interleaved with tool observations.  
*See:* file 11

**Rerank / reranker**  
Re-order retrieved candidates with a stronger (usually slower) model.  
*See:* file 08

**RLHF**  
Reinforcement Learning from Human Feedback — preference alignment method.  
*See:* file 05

**RRF (Reciprocal Rank Fusion)**  
Simple formula to merge multiple ranked lists.  
*See:* file 08

**Rollback**  
Return to previous prompt/model/config version after a bad release.

---

## S

**Sampling**  
Randomly choosing next tokens according to probabilities (vs always greedy).

**Schema validation**  
Checking JSON/tool args against a formal schema.

**Semantic cache**  
Cache answers for similar queries (risky if ACL/version ignored).

**SFT**  
Supervised Fine-Tuning.  
*See:* file 05

**SLO**  
Service Level Objective — target for reliability/latency/quality.

**Softmax**  
Turns scores into probabilities that sum to 1.  
*See:* file 03, 06

**Sparse retrieval**  
Keyword-style search (e.g. BM25), not dense vectors.

**Streaming**  
Sending tokens to the UI as they are generated.

**System prompt**  
Trusted instructions/policy message.

---

## T

**Temperature**  
Controls randomness of token sampling.  
*See:* file 06

**Token / tokenization**  
Model’s text units / the splitting process.  
*See:* file 01

**Top-k**  
Sample only from k best tokens.  
*See:* file 06

**Top-p (nucleus)**  
Sample from smallest set whose probs sum to ≥ p.  
*See:* file 06

**Tool runtime**  
Service that executes tool calls safely (auth, validate, timeout).

**Trajectory**  
Step-by-step record of an agent run.  
*See:* file 13

**Transformer**  
Neural architecture built from attention + feed-forward blocks.  
*See:* file 03

**TTFT**  
Time To First Token.  
*See:* file 04

---

## V

**Value (attention)**  
Content vector mixed in after Keys match the Query.  
*See:* file 03

**Vector**  
Ordered list of numbers (embedding).  
*See:* file 02

**vLLM**  
High-performance engine for serving LLMs (paging/continuous batching ideas).  
*See:* file 04, 10

---

## W

**Workflow / state machine**  
Explicit steps and transitions for business processes.  
*See:* file 12

---

## Extra production words you will hear

| Word | Easy meaning |
|------|----------------|
| **Canary** | small test rollout |
| **Chargeback** | attribute AI cost to teams |
| **Continuous batching** | pack many decode requests efficiently on GPU |
| **Degraded mode** | reduced features when systems are sick |
| **Escape hatch / kill switch** | emergency stop for agents/models |
| **Exactly-once / at-least-once** | delivery guarantees; at-least-once needs idempotency |
| **Feature flag** | switch behavior without full redeploy |
| **Grounding** | tying answers to sources |
| **Least privilege** | only the permissions needed |
| **Runbook** | step-by-step ops instructions for incidents |
| **Sandbox** | isolated environment for risky code execution |
| **Side effect** | real-world change (write/email/payment) |
| **Tenant** | customer/team isolation unit |

---

## Study tip

For every glossary word you don’t feel in your bones:

1. Read the linked knowledge file section  
2. Draw the ASCII diagram from the interview Q  
3. Say the one-sentence mastery line out loud  

---

*Glossary aligned to interview simulation 2026-07-18*
