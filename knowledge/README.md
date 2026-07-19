# Knowledge Base — LLM & Agentic Systems (from scratch)

**Purpose:** Fill every gap from the interview simulation  
`interview_sim_AI_Engineer_Enterprise_LLM_Agentic_2026-07-18.pdf`

**Style:** Very easy language · analogies · tiny math · beginner → advanced

---

## How to use this folder

1. Start at **Level 0** if terms feel fuzzy.
2. Read one file at a time. After each file, explain it out loud in 60 seconds.
3. Use **[GLOSSARY.md](GLOSSARY.md)** as a dictionary while reading the interview PDF.
4. When a diagram appears in the interview, open the matching knowledge file and redraw it yourself.

---

## Learning path (order)

| Level | File | What you learn |
|-------|------|----------------|
| 0 | [01_tokens_and_tokenization.md](01_tokens_and_tokenization.md) | What the model actually “eats” |
| 1 | [02_embeddings_and_similarity.md](02_embeddings_and_similarity.md) | Meaning as numbers + search |
| 2 | [03_attention_and_transformers.md](03_attention_and_transformers.md) | How “look back” works (+ math made easy) |
| 3 | [04_kv_cache_inference_latency.md](04_kv_cache_inference_latency.md) | Why long chats get slow/expensive |
| 4 | [05_training_sft_rlhf_dpo_lora.md](05_training_sft_rlhf_dpo_lora.md) | How models learn and get aligned |
| 5 | [06_decoding_temperature_topp_topk.md](06_decoding_temperature_topp_topk.md) | How the next word is chosen |
| 6 | [07_hallucination_and_evaluation.md](07_hallucination_and_evaluation.md) | Wrong-but-fluent answers + how to test |
| 7 | [08_rag_chunking_hybrid_search.md](08_rag_chunking_hybrid_search.md) | Enterprise Q&A architecture |
| 8 | [09_prompts_roles_injection_json.md](09_prompts_roles_injection_json.md) | Prompt design, attacks, structured output |
| 9 | [10_gateway_routing_cost.md](10_gateway_routing_cost.md) | Model routing, fallbacks, money |
| 10 | [11_agents_react_tools_memory.md](11_agents_react_tools_memory.md) | Agents vs chat, ReAct, tools, memory |
| 11 | [12_multi_agent_orchestration_hitl.md](12_multi_agent_orchestration_hitl.md) | Workflows, multi-agent, human approval |
| 12 | [13_reliability_idempotency_observability.md](13_reliability_idempotency_observability.md) | Retries, double-writes, debugging |
| 13 | [14_security_gdpr_eu_ai_act.md](14_security_gdpr_eu_ai_act.md) | Security + compliance in plain words |
| — | [GLOSSARY.md](GLOSSARY.md) | Every technical word, one-liners |

---

## 30-day weak-spot plan (practical)

| Days | Focus |
|------|--------|
| 1–3 | Tokens, embeddings, attention (files 01–03) |
| 4–5 | KV cache, decoding (04, 06) |
| 6–8 | Training/alignment + hallucination/evals (05, 07) |
| 9–12 | RAG end-to-end (08) + redraw interview Q14–Q15 |
| 13–15 | Prompts, injection, JSON, gateway (09–10) |
| 16–22 | Agents, tools, memory, HITL (11–12) |
| 23–26 | Idempotency, observability, security (13–14) |
| 27–30 | Re-read interview PDF; answer each Q with a diagram |

---

## Source

Concepts are taken from your AI Engineer enterprise interview simulation (LLM fundamentals → agentic systems) and related production topics (RAG, gateways, GDPR).

---

*Last updated: 18 July 2026*
