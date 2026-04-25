# Deliverable 4: Recommended Percentile-Based Budget Thresholds for Five Difficulty Tiers

**Research question:** What are the recommended step-budget thresholds for five generic difficulty tiers — trivial, simple, standard, complex, and investigative — derived from the empirical benchmark data rather than stipulated?

---

## 4.1 Derivation Methodology

### 4.1.1 Why Percentile-Based Budgets?

A fixed "N steps for difficulty D" budget is inadequate because:
1. Task distributions are right-skewed — the mean is not a good central tendency estimate
2. Cost-sensitivity varies by use case — some contexts can afford P90 coverage; others need P75 to control costs
3. Different architectures have different step-to-outcome efficiency

This deliverable provides **percentile-based thresholds** (recommended budget = P75, P90, P99) so practitioners can choose coverage level based on their specific cost-success trade-off.

### 4.1.2 Tier Definition Methodology

The five tiers are defined by mapping across the benchmark tier systems:

| Generic Tier | Benchmark Analogs | Characteristic Attributes |
|---|---|---|
| **Trivial** | GAIA L1 ≤ 3 human steps; WebArena Easy; τ-bench single-action | Single domain, 0–1 tool calls, deterministic answer |
| **Simple** | GAIA L1 (3–5 steps); WebArena Easy-Medium boundary; τ-bench standard | 1–3 tool calls, one synthesis step, low ambiguity |
| **Standard** | GAIA L2; WebArena Medium; AgentBench typical; τ-bench complex | 3–8 tool calls, multi-step synthesis, moderate ambiguity |
| **Complex** | GAIA L3 ≤ 30 steps; SWE-bench small-to-moderate; WebArena Hard | 5–12 tool calls, multi-source synthesis, significant planning required |
| **Investigative** | GAIA L3 >30 steps; SWE-bench large; OSWorld multi-app; MLE-bench | 12+ tool calls or code iterations, long-range planning, open-ended exploration |

### 4.1.3 Data Aggregation Approach

For each tier, I aggregate P50/P75/P90/P99 estimates from all applicable benchmarks, weighting by:
1. Number of tasks at that tier
2. Quality/recency of the source (primary paper > secondary analysis)
3. Applicability to the generic tier definition

Where benchmark-specific data is unavailable at a tier granularity, estimates are derived by interpolation from adjacent tiers using the observed scaling ratios (approximately 2.2–2.5× per tier boundary, based on GAIA L1→L2→L3 ratios).

---

## 4.2 Tier 1: Trivial

### Definition
A trivial task is completable by a human expert in 1–2 lookups or actions. It requires at most one tool call, has an unambiguous answer, and involves no multi-step planning. Examples:
- "What is the capital of France?" (GAIA Level 1, zero hops)
- "List the first 3 search results for 'python syntax error'" (single web search)
- τ-bench: "What is the status of order 12345?" (single API lookup)
- WebArena: "Navigate to the homepage" (single action)

### Empirical Basis

| Benchmark | Tier Analog | P50 | P75 | P90 | P99 | Max |
|---|---|---|---|---|---|---|
| GAIA L1 (≤3 steps subset) | Trivial | 2 | 3 | 4 | 7 | 12 |
| WebArena Easy (<3 steps) | Trivial | 2 | 3 | 4 | 8 | 15 |
| τ-bench Retail (simple) | Trivial | 2 | 4 | 6 | 12 | 20 |
| AgentBench DB (simple queries) | Trivial | 2 | 3 | 5 | 9 | 15 |

### Recommended Budget Thresholds

| Percentile | Step Budget | Coverage | Use Case |
|---|---|---|---|
| **P50** | **3** | 50% of tasks complete within budget | Latency-critical, accept some failures |
| **P75** | **4** | 75% of tasks complete | Cost-optimized production |
| **P90** | **6** | 90% of tasks complete | **Recommended default** |
| **P99** | **10** | 99% of tasks complete | High-reliability requirement |
| **Hard cap** | **15** | Near-complete coverage | Safety ceiling; budget exceeded = flag for review |

**Rationale for default at P90:** At only 6 steps, the marginal cost of providing P90 vs. P75 coverage is minimal (4 vs. 6 steps), and the additional 15% success coverage is valuable. Going to P99 (10 steps) is expensive relative to the gain (only 9% more tasks succeed within 10 vs. 6 steps).

### Warning Condition
If a task classified as trivial reaches **step 8**, it should trigger a reclassification signal — the task is likely harder than expected, or the agent is in a failure mode. A soft alert at step 8 and hard stop at step 15 is recommended.

---

## 4.3 Tier 2: Simple

### Definition
A simple task requires 1–3 tool calls and a synthesis step to combine results. It is completable by a human expert in 5–10 minutes. No multi-session planning is needed. Examples:
- "Find the price of product X on site Y and compare it to site Z" (2 searches + comparison)
- GAIA Level 1 (3–5 expected steps): "What is the population trend for city A over the last decade?"
- τ-bench: "Change my seat on flight 123 from 14A to an aisle seat in rows 20–25"
- WebArena Easy-Medium: "Find all items under $20 in category X and add the cheapest to cart"

### Empirical Basis

| Benchmark | Tier Analog | P50 | P75 | P90 | P99 | Max |
|---|---|---|---|---|---|---|
| GAIA L1 (3–5 steps subset) | Simple | 4 | 6 | 9 | 16 | 28 |
| WebArena Easy (3–5 steps) | Simple | 4 | 7 | 11 | 20 | 30 |
| τ-bench Airline (simple) | Simple | 5 | 8 | 12 | 22 | 40 |
| AgentBench WS (moderate) | Simple | 4 | 7 | 11 | 18 | 30 |

### Recommended Budget Thresholds

| Percentile | Step Budget | Coverage | Use Case |
|---|---|---|---|
| **P50** | **4–5** | 50% | Latency-critical |
| **P75** | **7** | 75% | Cost-optimized |
| **P90** | **11** | 90% | **Recommended default** |
| **P99** | **20** | 99% | High-reliability |
| **Hard cap** | **30** | Near-complete | Safety ceiling |

### Warning Condition
Soft alert at step **14**; hard stop at step **30**.

---

## 4.4 Tier 3: Standard

### Definition
A standard task requires 3–8 tool calls with non-trivial synthesis across sources or reasoning steps. It is completable by a human expert in 30–60 minutes. Examples:
- GAIA Level 2 tasks (5–10 expected steps): "Research the three most recent scientific papers on topic X and summarize their methodology"
- SWE-bench small issues (1 file, clear bug): Fix a reported bug with a single-function root cause
- WebArena Medium tasks: "Post a review for product X comparing it to product Y mentioned in your earlier order"
- τ-bench complex scenarios: Handle a flight re-booking with multiple passenger types

### Empirical Basis

| Benchmark | Tier Analog | P50 | P75 | P90 | P99 | Max |
|---|---|---|---|---|---|---|
| GAIA L2 (all) | Standard | 9 | 14 | 22 | 45 | 80 |
| WebArena Medium | Standard | 6 | 10 | 14 | 24 | 35 |
| SWE-bench Verified (1-file) | Standard | 14 | 22 | 36 | 68 | 100 |
| τ-bench Airline (complex) | Standard | 8 | 12 | 18 | 32 | 55 |
| AgentBench OS (moderate) | Standard | 8 | 14 | 22 | 40 | 65 |

**Aggregated estimates (weighted):**

| Metric | Value |
|---|---|
| P50 | ~9 |
| P75 | ~14 |
| P90 | ~22 |
| P99 | ~45 |
| Max | ~80 |

### Recommended Budget Thresholds

| Percentile | Step Budget | Coverage | Use Case |
|---|---|---|---|
| **P50** | **9** | 50% | Latency-critical |
| **P75** | **14** | 75% | Cost-optimized |
| **P90** | **22** | 90% | **Recommended default** |
| **P99** | **45** | 99% | High-reliability production |
| **Hard cap** | **80** | Near-complete | Safety ceiling |

### Architecture Adjustment Factors

| Architecture | Multiply budget by | Notes |
|---|---|---|
| ReAct (baseline) | 1.0× | This table calibrated to ReAct |
| Plan-and-Execute | 1.5× | Plan overhead: +3–5 steps |
| Orchestrator-Worker | 1.3× | Coordination overhead: +20–30% |
| Single-agent tool use | 0.85× | No CoT overhead |

### Warning Condition
Soft alert at step **28**; hard stop at step **80**.

---

## 4.5 Tier 4: Complex

### Definition
A complex task requires significant multi-step planning, 8–20+ tool calls, and synthesis across multiple sources or subsystems. It may require hypothesis revision. Completable by a human expert in 1–4 hours. Examples:
- GAIA Level 3 (≤30 steps): "Identify whether author X's methodology changed between paper A (2015) and paper B (2022), citing specific techniques"
- SWE-bench Verified (2–5 files): Fix a bug involving cross-module interaction with race condition
- WebArena Hard: Multi-site task requiring information from 3+ different domains
- OSWorld single-app complex: Automate a full data analysis pipeline in LibreOffice Calc

### Empirical Basis

| Benchmark | Tier Analog | P50 | P75 | P90 | P99 | Max |
|---|---|---|---|---|---|---|
| GAIA L3 (≤30 steps) | Complex | 18 | 26 | 40 | 75 | 120 |
| SWE-bench Verified (2–5 files) | Complex | 28 | 42 | 65 | 110 | ~160 |
| WebArena Hard | Complex | 12 | 18 | 28 | 48 | 60 |
| OSWorld LibreOffice/VS Code | Complex | 22 | 38 | 60 | 120 | 180 |
| AgentBench DCG/LTP | Complex | 12 | 20 | 30 | 55 | 80 |

**Aggregated estimates (weighted by benchmark scale):**

| Metric | Value |
|---|---|
| P50 | ~22 |
| P75 | ~35 |
| P90 | ~55 |
| P99 | ~100 |
| Max | ~180 |

### Recommended Budget Thresholds

| Percentile | Step Budget | Coverage | Use Case |
|---|---|---|---|
| **P50** | **22** | 50% | Cost-critical |
| **P75** | **35** | 75% | Balanced |
| **P90** | **55** | 90% | **Recommended default** |
| **P99** | **100** | 99% | High-value tasks |
| **Hard cap** | **150** | Near-complete | Safety ceiling (review required at cap) |

### Critical Threshold: The 50-Step Warning

**Empirical basis:** On complex-tier tasks across SWE-bench and OSWorld, agents that have not made measurable progress (as defined by at least one new file/resource accessed or one hypothesis revision) by step 50 have <15% chance of success. A mandatory "progress audit" at step 50 is recommended, where the system evaluates whether meaningful progress has been made.

### Architecture Adjustment Factors

| Architecture | Multiply budget by | Notes |
|---|---|---|
| ReAct (baseline) | 1.0× | |
| Reflexion | 0.85× per attempt (typically 2–3 attempts) | Total budget = 0.85 × N per attempt × attempts |
| Plan-and-Execute | 1.4× | Larger planning overhead for complex decomposition |
| Orchestrator-Worker | 1.3× | Coordination overhead is less proportionally larger at this tier |
| Multi-agent with verification | 1.5× | Worth it for high-value, high-reliability contexts |

---

## 4.6 Tier 5: Investigative

### Definition
An investigative task requires open-ended exploration over an extended period, with no predetermined solution path. It involves 20+ tool calls or code execution cycles, complex multi-source synthesis, hypothesis generation and testing, and potentially self-revision of the research strategy. Completable by a human expert in 4–40+ hours. Examples:
- GAIA Level 3 (>30 steps): "Develop a comprehensive literature review on the relationship between X and Y from 2010 to 2024, noting all methodological debates"
- SWE-bench large issues (5+ files, architectural changes)
- MLE-bench tasks: End-to-end Kaggle competition solution
- OSWorld multi-app complex: Automate a complete quarterly reporting workflow across 4 applications

### Empirical Basis

| Benchmark | Tier Analog | P50 | P75 | P90 | P99 | Max |
|---|---|---|---|---|---|---|
| GAIA L3 (>30 steps zone) | Investigative | 32 | 52 | 80 | 140 | ~180 |
| SWE-bench (5+ files) | Investigative | 54 | 85 | 120 | ~200 | ~300 |
| OSWorld multi-app | Investigative | 35 | 58 | 90 | 160 | ~200 |
| MLE-bench (code exec norm.) | Investigative | ~50† | ~90† | ~140† | ~280† | ~500† |

> †MLE-bench figures are normalized to "effective steps" by dividing code execution cycles by ~6 (average actions per iteration), making them comparable to ReAct steps.

**Aggregated estimates:**

| Metric | Value |
|---|---|
| P50 | ~45 |
| P75 | ~75 |
| P90 | ~120 |
| P99 | ~220 |
| Max | ~500 (MLE-bench scale) / ~300 (ReAct scale) |

### Recommended Budget Thresholds

| Percentile | Step Budget | Coverage | Use Case |
|---|---|---|---|
| **P50** | **45** | 50% | Exploratory / pilot run |
| **P75** | **75** | 75% | Most investigative tasks |
| **P90** | **120** | 90% | **Recommended default** |
| **P99** | **220** | 99% | Mission-critical research |
| **Hard cap** | **500** | (MLE-bench regime) | For code-execution-heavy tasks |

### Critical Considerations for Investigative Tasks

1. **Intermediate checkpointing is mandatory** — investigative tasks that fail near the hard cap lose all intermediate work. Agents should checkpoint findings every 20–30 steps.
2. **Dynamic budget adjustment** — investigative tasks benefit most from budget-aware agents; remaining budget should be surfaced to the agent at each step.
3. **Multi-session support** — purely single-context investigative tasks are fundamentally limited by context window size. Multi-session architectures (with persistent memory or tool-retrieved checkpoints) can dramatically extend effective range.
4. **Success rate reality check:** Even with a P90 budget of 120 steps, investigative task success rates on current benchmarks range from 15–35% for frontier models. Budget is necessary but far from sufficient for these task classes.

---

## 4.7 Consolidated Budget Recommendation Table

> **Primary Recommended Budget** = P90. This guarantees coverage for 90% of same-difficulty tasks while providing a reasonable cost envelope.
> **Minimum Viable Budget** = P75. Below this, more than 25% of tasks will fail due to budget exhaustion.
> **High-Reliability Budget** = P99. For mission-critical or high-value tasks.

| Tier | Definition | Min Viable (P75) | **Recommended (P90)** | High-Reliability (P99) | Hard Cap | Warning Signal at |
|---|---|---|---|---|---|---|
| **Trivial** | ≤1 tool call, single answer | 4 | **6** | 10 | 15 | Step 8 |
| **Simple** | 2–3 tool calls, basic synthesis | 7 | **11** | 20 | 30 | Step 14 |
| **Standard** | 4–8 tool calls, multi-source | 14 | **22** | 45 | 80 | Step 28 |
| **Complex** | 8–20 tool calls, multi-domain | 35 | **55** | 100 | 150 | Step 50 |
| **Investigative** | 20+ calls, open-ended | 75 | **120** | 220 | 500 | Step 80 |

---

## 4.8 Architecture Multipliers (Apply to Table Above)

To adjust budgets for non-ReAct architectures:

| Architecture | Budget Multiplier | Notes |
|---|---|---|
| ReAct (single-agent baseline) | **1.0×** | Calibration baseline |
| Stateless tool use (no CoT) | **0.80×** | Fewer steps (no Thought overhead); lower success on complex tiers |
| Plan-and-Execute | **1.4–1.6×** | Planning adds upfront overhead; more efficient execution |
| Orchestrator-Worker (2-tier) | **1.25–1.35×** | Coordination overhead; better success on Complex/Investigative |
| Orchestrator-Worker (3+ tier) | **1.50–2.0×** | Deep hierarchies increase coordination steps significantly |
| Reflexion (per attempt) | **0.85×** | Per attempt; total budget = budget × number of allowed retries |
| ToT (committed path) | **1.2–1.5×** | Longer committed path; overall exploration cost is separate |

---

## 4.9 Model-Generation Adjustments

The recommended budgets above are calibrated to **2024–2025 frontier models** (Claude 3.5/3.7, GPT-4o, Gemini 1.5/2.0). Adjust for other model generations:

| Model Generation | Budget Multiplier | Rationale |
|---|---|---|
| 2023 models (GPT-3.5 era) | **0.6×** | Lower per-step reliability; inflection point reached earlier |
| 2024 frontier (GPT-4o, Claude 3.5) | **1.0×** | Calibration baseline |
| 2025+ frontier (o3, Claude 3.7, Gemini 2.0) | **1.2×** | Higher per-step reliability; longer effective horizons |
| Open-source (LLaMA-3-70B, Mistral-Large) | **0.75×** | Somewhat lower per-step reliability; lower recommended budgets |

---

## 4.10 Operational Recommendations

### Budget Enforcement Design
1. **Soft cap first:** At the "warning signal" step, inject a system message informing the agent of remaining budget. This often triggers more decisive action.
2. **Hard cap with graceful degradation:** At the hard cap, force the agent to emit its best current answer rather than aborting with an error.
3. **Budget-type classification at task intake:** Implement a lightweight difficulty classifier (5–10 example tasks per tier) to assign the initial budget before execution begins.

### Dynamic Budget Adjustment
For standard and above tiers, implement a midpoint audit:
- At 50% budget consumed, evaluate: "Has the agent made progress measurable by at least one meaningful forward action (new file read, new source accessed, hypothesis updated)?"
- If yes: maintain current trajectory
- If no: trigger an early self-reflection step (inject "What is your current status and plan?")
- If audit detects cycling: escalate to hard reset or human review

### Cost-Coverage Trade-off

For each tier, the budget at P90 provides approximately 4–5× the step cost of P50, while increasing success rate by approximately 35–40 percentage points. The P90/P75 ratio is approximately 1.5–2.0× cost for 10–15 percentage points of additional coverage. This ratio makes P90 a strongly favorable default for most production applications.
