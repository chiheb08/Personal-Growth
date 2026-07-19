# 10 — LLM Gateway, Routing, Cost

**Interview links:** Q19, Q41–Q42 · production platform thinking

---

## Level 0 — Why not call the vendor from every service?

If 12 microservices each have their own API key and logic:

- outages are chaos  
- cost is invisible  
- fallbacks are inconsistent  
- security review is painful  

A **gateway** is the front door for model calls.

**Analogy:**  
One airport security + routing hub, not 12 private airstrips.

---

## Level 1 — What a gateway does

```
App --> LLM GATEWAY --> model A / model B / on-prem vLLM
              |
              +-- API keys
              +-- rate limits
              +-- routing rules
              +-- fallbacks
              +-- logging (tokens, cost, team)
```

Examples of gateway-style layers: LiteLLM-like proxies, internal AI platforms.

---

## Level 2 — Routing rules (enterprise)

Route by:

1. **Sensitivity** — PII/secrets → on-prem only  
2. **Task difficulty** — FAQ → small/cheap model; hard reasoning → large model  
3. **Latency SLO** — interactive vs batch  
4. **Cost budget** — stop runaway spend  
5. **Health** — on 429/5xx → fallback model  

```
if classified_sensitive: on_prem
elif intent == "faq": small_model
elif need_deep_reason: large_model
if upstream_429: fallback
```

---

## Level 3 — Cost math (easy)

```
monthly_cost ≈ sum over requests (tokens_in + tokens_out) × price
```

Agent loops multiply this:

```
cost ≈ steps × tokens_per_step × price
```

So **max steps** is a financial control, not only a UX control.

---

## Level 4 — Latency vs cost trade-offs

| Move | Effect |
|------|--------|
| Smaller model for routing/classify | cheaper/faster |
| Cache repeated queries | less spend |
| Fewer retrieved chunks | less tokens |
| Parallel independent tool calls | lower latency |
| Stream tokens / status | better perceived wait |

---

## Level 5 — Fallbacks and degrade modes

```
primary model unhealthy
   --> secondary model
   --> cached FAQ answers only
   --> honest "degraded" message
```

Never “wait forever for OpenAI to recover” as your only plan.

---

## Self-check

1. Name 3 reasons for a gateway.  
2. Why route PII on-prem?  
3. Why do agents need max steps for cost?

**Answers:** 1) keys/limits/fallback/cost. 2) residency/policy. 3) loops multiply tokens.

---

## One-sentence mastery line

> A gateway turns “random model calls” into a managed platform: route, limit, fallback, and account for every token.
