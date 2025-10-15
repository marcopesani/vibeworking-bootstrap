# Learn how to understand the differences in AI models’ capabilities

This guide teaches non‑technical decision makers how to compare AI models. It focuses on the few levers that change outcomes in production: model size, context window, reasoning controls, multimodality, tool calling, structured output, and safety. It includes examples, checklists, and cost math.

## 1) Model size (capacity)

**What it is.** Roughly how many parameters the model has. You will often see public tags like **8B**, **70B**, or **~500B**. Bigger usually means better generalization and instruction following, but higher cost and latency.

**Why it matters.**

* Quality: larger models handle edge cases and follow nuanced instructions better.
* Cost: more compute per token.
* Latency: higher time‑to‑first‑token (TTFT) and lower throughput.

**Heuristic.**

* Start small for simple classification, extraction, short summaries.
* Use medium for customer support, long context Q&A, document analysis.
* Use large only when smaller models fail acceptance tests.

**Example mental model.**

* **8B** → fast, cheap, good for deterministic tasks with strong prompts or tools.
* **70B** → strong generalist for most enterprise tasks.
* **≈500B** → best raw capability; use when accuracy is business‑critical and budget allows.

## 2) Context window and output limits

**What it is.** The **context window** is how many tokens the model can read at once. Common ranges:

* **128k tokens** (large enough for a book‑length document)
* **1M tokens** (large corpora or multi‑file projects)

**Output limit** is how many tokens a single response may generate.

**Why it matters.**

* Long documents: fewer chunks, less prompt engineering.
* Complex tasks: keep more relevant facts in memory at once.

**Token basics.**

* 1 token ≈ 3–4 English characters ≈ 0.75 words, on average.
* **Input tokens** are what you send in (prompt + documents). **Output tokens** are what the model writes back.
* Providers usually price these differently. Output almost always costs more.

**Quick examples.**

* 128k input window → legal brief or long technical spec in one pass.
* 1M input window → entire wiki + codebase index in a single prompt.

**Rule of thumb.** If your source material exceeds the window, plan for retrieval (RAG) or task decomposition.

## 3) Reasoning controls

**What it is.** Settings or modes that allow the model to "think" longer or in different ways (e.g., deeper chains of thought, self‑consistency, or tool‑assisted reasoning). Some vendors expose knobs like *thinking*, *deep think*, or adaptive reasoning budgets.

**Why it matters.**

* Higher reasoning budgets improve accuracy on complex problems but raise latency and cost.
* Switching modes per task can beat a permanently large model on price‑performance.

**How to use.**

* Default to standard mode. Enable extended reasoning only for tasks that fail acceptance tests (math proofs, multi‑step planning, tricky extractions).
* Set time or token caps so invoices remain predictable.

## 4) Multimodality (text, images, audio, video)

**What it is.** Ability to ingest or generate across modalities.

**Why it matters.**

* Images: diagram understanding, UI parsing, receipts, screenshots.
* Audio: call analysis, voice agents, meeting notes.
* Video: compliance review, content moderation, instructional design.

**Check.** Confirm which directions are supported: text→image understanding, image→text, audio→text, text→audio, video→text, and whether generation is available or only analysis.

## 5) Tool calling (function calling)

**What it is.** The model can call your functions or APIs during a run: databases, search, calculators, CRMs, ticketing systems.

**Why it matters.**

* Shifts the model from “answering” to **acting**.
* Reduces hallucinations by delegating facts to tools.

**Design patterns.**

* Deterministic tools for math, lookup, CRUD operations.
* Guardrail tools for policy checks before executing side effects.
* Observability: log tool inputs/outputs and add retry/backoff.

## 6) Structured output

**What it is.** The model returns well‑formed JSON (or other schemas) you can parse safely.

**Why it matters.**

* Enables automation without brittle regex post‑processing.
* Supports contracts with downstream systems.

**Reliability tips.**

* Provide a JSON Schema with examples.
* Stream partial JSON only if the consumer supports it.
* Add server‑side validators and automatic repair prompts for malformed outputs.

## 7) Safety and control

**What it is.** Content filters, instruction adherence, red‑teaming, audit logs, and tenant isolation.

**Why it matters.**

* Enterprise risk, compliance, and reputational safety.

**Ask vendors.**

* What categories are blocked by default? Can we tune them?
* How are incidents logged and surfaced?
* What data is stored, for how long, and where?

## Practical comparisons

### A. Lightweight scoring rubric

Score each criterion 1–5 for your use case, then compute a weighted average.

| Criterion                    | Weight | Notes                                     |
| ---------------------------- | -----: | ----------------------------------------- |
| Accuracy on acceptance tests |   0.30 | Your gold‑standard tasks trump benchmarks |
| Cost per task                |   0.20 | See token math below                      |
| Latency                      |   0.15 | Measure P50 and P95                       |
| Context suitability          |   0.15 | Does it fit your documents?               |
| Tool/structured output fit   |   0.15 | JSON conformance + tool success rate      |
| Safety/control               |   0.05 | Meets policy and audit needs              |

### B. Example profiles

* **8B with tools**: lowest cost for extractions, routing, numeric work. Needs clear prompts and good tools.
* **70B generalist**: strong default for support copilots, analytics summaries, doc QA.
* **≈500B with extended reasoning**: best shot at complex, high‑stakes reasoning. Use only when justified by business impact.

### C. Context window choices

* If documents ≤ **128k**, a large window model simplifies MVPs.
* If documents often > **128k**, favor **RAG** + smaller model or a **1M** window model for simplicity.

## Cost and latency: token math you will actually use

### 1) Estimate tokens

* English text: **~0.75 words per token** (approximation).
* 1,000 words ≈ **1,300 tokens**. 20 pages ≈ **20k–30k tokens** depending on density.

### 2) Compute cost per run

```
Cost ≈ (InputTokens / 1,000,000) × Price_in  +  (OutputTokens / 1,000,000) × Price_out
```

**Example.** You send 120k tokens (one long report). You request up to 6k output tokens.

* If Price_in = $1.25/M and Price_out = $10/M:

  * Input: 120k × $1.25/M = **$0.15**
  * Output: 6k × $10/M = **$0.06**
  * **Total ≈ $0.21** per run

### 3) Latency budget

* TTFT grows with model size and reasoning depth.
* Batch where possible. Use streaming to mask latency for humans.
* Cap reasoning tokens or steps for live UX.

## Evaluation protocol that works

1. **Define 20–50 acceptance tests** representing real tasks.
2. **Blind A/B** at the prompt level. Keep temperature and reasoning settings fixed.
3. Track: exact‑match or rubric score, JSON validity rate, tool call success, latency P50/P95, and cost per task.
4. Add a **hallucination check**: a verifier tool or cross‑model adjudication on critical facts.
5. Re‑test monthly. Vendors ship updates that can shift performance.

## Selection playbooks by use case

### Customer support copilot

* Start: medium model, 128k window.
* Add tools: knowledge search, ticket lookup, order operations.
* JSON outputs for suggested replies and action summaries.
* Guardrails for PII leakage and policy violations.

### Document analytics and extraction

* If docs fit 128k: single‑pass parsing with schema‑guided JSON.
* If not: RAG + chunking or a 1M window.
* Validate JSON server‑side and auto‑repair on error.

### Code and data assistants

* Favor models with strong reasoning controls and longer outputs.
* Tools: repo search, linters, SQL runners, calculators.
* Enforce schemas for plans and diffs.

### Voice or meeting AI

* Require audio I/O and diarization.
* Summaries with structured action items; tool call to calendar/CRM.
* Latency caps and graceful fallbacks in real time.

## What to ask vendors

* **Context**: maximum input and output tokens in one call? Any differences per endpoint?
* **Reasoning**: how do I control depth, and what is the cost impact?
* **Multimodal**: which directions are supported? Any size/time limits?
* **Tools**: function calling limits, parallel calls, retries, and timeouts.
* **Structured output**: native JSON Schema support? Repair modes?
* **Pricing**: exact $/M for input vs output, and for high‑context or reasoning modes.
* **Data**: retention, residency, opt‑out, and isolation details.

## Common pitfalls and fixes

* **JSON breakage** → Provide schema and examples; validate and auto‑repair.
* **Context overflow** → Trim with retrieval; summarize before generation.
* **Hallucinations** → Route facts to tools; verify critical outputs.
* **Unbounded cost** → Cap output tokens; limit reasoning; pre‑summarize inputs.
* **Slow UX** → Stream output; pre‑compute; batch; cache embeddings and KV state.

## Glossary

* **Token**: smallest unit of text a model processes. Billing unit.
* **Context window**: max tokens the model can read per call.
* **Output limit**: max tokens the model can write per call.
* **Reasoning budget**: allowance of internal steps/tokens for better accuracy.
* **Tool calling**: the model invokes your functions/APIs.
* **Structured output**: machine‑readable response conforming to a schema.

## One‑page checklist

* [ ] Context window fits my documents (e.g., 128k vs 1M)
* [ ] Output limit fits my deliverables
* [ ] Reasoning control available and capped
* [ ] Multimodal directions I need are supported
* [ ] Tool calling with clear contracts and timeouts
* [ ] Structured output with JSON Schema and server validation
* [ ] Safety controls meet policy
* [ ] Cost per task modeled for input vs output
* [ ] Evaluation set built and automated

### Final takeaway

Pick the **smallest** model that passes your **acceptance tests**, then scale up **reasoning** or **window** only when the task demands it. Control **output tokens** and enforce **structured outputs** to keep costs predictable and automation reliable.
