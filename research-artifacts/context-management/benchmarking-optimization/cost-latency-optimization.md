# Cost-Latency Optimization for LLM Inference

## Introduction

LLM inference cost and latency optimization has transitioned from an academic exercise to a core business discipline, with enterprise AI budgets frequently dominated by inference costs that can reach millions of dollars per month at scale. The fundamental challenge is navigating the "infeasibility triangle" of latency, throughput, and cost — improvements to any one dimension typically require trade-offs in the others. This report provides a comprehensive analysis of the full optimization stack for LLM inference, from model-level techniques (quantization, distillation, architecture selection) through system-level optimizations (serving engines, batching, memory management) to application-level strategies (routing, caching, prompt optimization), with a framework for Pareto-optimal deployment configuration that maximizes value within budget constraints.

---

## 1. Theoretical Foundations: The Cost-Latency Landscape

### 1.1 Cost Drivers in LLM Inference

LLM inference costs are primarily determined by four factors:

- **Hardware Cost:** GPU/accelerator rental or amortized purchase cost. GPU-hours are the fundamental billing unit for self-hosted deployments.
- **Token Volume:** Input and output token counts determine compute requirements. Output tokens are 3-8x more expensive than input tokens because generation is sequential (autoregressive) while input processing is parallelizable.
- **Model Size:** Larger models require more GPU memory and compute per token. A 70B parameter model costs roughly 10x more per token than a 7B parameter model.
- **Utilization Efficiency:** GPU utilization during inference is typically 30-60% without optimization, meaning 40-70% of hardware cost is wasted on idle compute.

### 1.2 Latency Decomposition

| Metric | Definition | Typical Range | Bottleneck |
|---|---|---|---|
| **TTFT** | Time-to-First-Token | 50-2000ms | Prefill compute |
| **TPOT** | Time Per Output Token | 10-50ms | Memory bandwidth |
| **E2E Latency** | Total response time | 0.5-30s | TTFT + (TPOT × output_length) |
| **Queue Wait** | Time before processing starts | 0-5000ms | System load |

### 1.3 The Pareto Frontier

All optimization techniques can be mapped onto a cost-latency Pareto frontier — the set of configurations where neither cost nor latency can be improved without degrading the other. The goal of optimization is to push this frontier toward the origin (lower cost AND lower latency) through stacked optimizations.

---

## 2. Model-Level Optimizations

### 2.1 Quantization

The highest-impact single optimization for cost reduction:

| Precision | Memory Reduction | Quality Impact | Throughput Gain | Best For |
|---|---|---|---|---|
| **FP16 (baseline)** | 1x | None | Baseline | Quality-critical apps |
| **FP8** | 2x | <1% quality loss | 1.5-2x | Production standard |
| **INT8 (W8A8)** | 2x | 1-2% quality loss | 1.5-2x | Cost-sensitive production |
| **INT4 (AWQ/GPTQ)** | 4x | 2-5% quality loss | 2-3x | Edge deployment, high volume |
| **GGUF (mixed)** | 3-5x | 1-4% quality loss | 2-4x | CPU/hybrid inference |

**Production Recommendation:** FP8 quantization is now the industry default — it halves memory requirements (enabling smaller/fewer GPUs) with negligible quality impact. INT4 is viable for high-volume, cost-sensitive workloads where 2-5% quality degradation is acceptable.

### 2.2 Model Selection and Right-Sizing

The most effective cost optimization is using the smallest model that meets quality requirements:

- **Frontier Models (200B+ params):** Reserve for complex reasoning, creative tasks, and quality-critical applications. Cost: $10-60 per million output tokens.
- **Mid-Tier Models (30-70B params):** Suitable for most enterprise tasks — summarization, analysis, code generation. Cost: $1-5 per million output tokens.
- **Small Models (7-13B params):** Excellent for routine tasks — classification, extraction, simple QA. Cost: $0.10-0.50 per million output tokens.
- **Distilled/Nano Models (<7B params):** For high-volume, simple tasks — intent classification, entity extraction, formatting. Cost: $0.01-0.10 per million output tokens.

### 2.3 Mixture-of-Experts (MoE) Architecture

MoE models activate only a subset of parameters per token (e.g., Mixtral 8x7B activates 2 of 8 experts), providing quality competitive with dense models at 2-4x lower compute cost per token. The trade-off is higher memory requirement (all expert weights must be loaded).

---

## 3. System-Level Optimizations

### 3.1 Serving Engine Selection

| Engine | Key Feature | Throughput | Latency | Maturity |
|---|---|---|---|---|
| **vLLM** | PagedAttention, continuous batching | Excellent | Good | Production standard |
| **SGLang** | RadixAttention, constrained decoding | Excellent | Excellent | Production-ready |
| **TensorRT-LLM** | NVIDIA-optimized kernels | Best (NVIDIA) | Excellent | Production standard |
| **llama.cpp** | CPU/hybrid inference | Moderate | Good | Production (edge) |
| **MLC-LLM** | Cross-platform compilation | Good | Good | Maturing |

### 3.2 Batching Strategies

**Continuous Batching:** Dynamically adds new requests to the batch as existing requests complete, maintaining high GPU utilization. The industry standard implemented in all major serving engines.

**Chunked Prefill:** Breaks long prefill operations into smaller chunks, interleaving them with decode operations from other requests. This prevents long prefills from causing latency spikes for other requests in the batch.

**Disaggregated Prefill/Decode:** Routing prefill-heavy and decode-heavy workloads to separately optimized GPU pools. Prefill benefits from tensor parallelism (compute-bound); decode benefits from pipeline parallelism (memory-bandwidth-bound).

### 3.3 KV Cache Optimization

- **PagedAttention:** 2-4x memory efficiency through virtual memory management.
- **Prefix Caching:** Reuse KV cache for shared prefixes. Up to 90% TTFT reduction for repeated system prompts.
- **KV Cache Quantization:** Compress KV cache to FP8/INT8, doubling effective cache capacity with minimal quality impact.

### 3.4 Speculative Decoding

Using a fast draft model to predict tokens, verified by the target model:
- **EAGLE/Medusa:** 2-3x decode speedup with zero quality impact.
- **Best for:** Latency-sensitive applications with moderate batch sizes.
- **Limitation:** Diminishing returns at very large batch sizes where GPU is compute-saturated.

---

## 4. Application-Level Optimizations

### 4.1 Intelligent Request Routing

The highest-impact application-level optimization:

**Complexity-Based Routing:**
- Classify each request by complexity (simple/moderate/complex)
- Route simple requests to small, cheap models
- Route complex requests to large, capable models
- Result: 50-80% cost reduction with <5% quality impact for well-calibrated routers

**Cascading:**
- Try the cheapest model first
- If confidence is below threshold, escalate to the next model
- Continue until confidence threshold is met or the most expensive model is reached
- Result: Average cost approaches the cheapest model while quality approaches the best model

### 4.2 Prompt and Context Optimization

**Prompt Caching:** Cache system prompts and common prefixes. Major cloud providers offer 50-90% input token discounts for cached prefixes.

**Context Compression:** Apply LLMLingua or similar compression to reduce input token count by 2-10x before inference.

**Output Length Control:** Constrain output length via max_tokens and structured output formats. Since output tokens cost 3-8x more than input tokens, reducing output verbosity has outsized cost impact.

### 4.3 Semantic Caching

Cache responses to semantically similar queries:
- Hash or embed each query
- Before inference, check if a semantically similar query has been answered recently
- If cache hit, return cached response (zero inference cost)
- Effective for FAQ-heavy workloads, achieving 30-60% cache hit rates

---

## 5. Comprehensive Optimization Stack

### 5.1 Cumulative Impact Analysis

| Optimization | Cost Reduction | Latency Reduction | Cumulative Cost | Implementation Effort |
|---|---|---|---|---|
| **Baseline (no optimization)** | 0% | 0% | $100 | None |
| **+ Model right-sizing** | 60-80% | 0-20% | $20-40 | Low |
| **+ FP8 quantization** | 40-50% | 10-20% | $10-24 | Low |
| **+ Efficient serving engine** | 30-50% | 30-50% | $5-17 | Moderate |
| **+ Speculative decoding** | 0-10% | 40-60% | $4.5-17 | Moderate |
| **+ KV cache optimization** | 10-20% | 20-40% | $3.6-15 | Low |
| **+ Prompt caching** | 20-50% (input) | 30-50% (TTFT) | $2.5-12 | Low |
| **+ Request routing** | 30-50% | Varies | $1.3-8.4 | Moderate |
| **+ Semantic caching** | 20-40% | 90%+ (hits) | $0.8-6.7 | Moderate |

**Fully optimized cost: 1-7% of unoptimized baseline.** A workload costing $100K/month unoptimized can be reduced to $1-7K/month with the full optimization stack.

### 5.2 Decision Framework

For each deployment, follow this priority order:
1. **Model selection** — Choose the smallest model that meets quality requirements
2. **Quantization** — Apply FP8 or INT8 quantization
3. **Serving engine** — Deploy on vLLM, SGLang, or TensorRT-LLM
4. **KV cache optimization** — Enable PagedAttention and prefix caching
5. **Speculative decoding** — Add if latency is critical
6. **Request routing** — Implement if multiple models are available
7. **Prompt optimization** — Compress context, control output length
8. **Semantic caching** — Implement for repetitive workloads

---

## 6. Challenges and Future Directions

### 6.1 Inference-Time Scaling

**Challenge:** "Reasoning" models that adaptively allocate compute during inference (variable-length chain-of-thought) create unpredictable cost and latency profiles. A single query might generate 100 or 10,000 reasoning tokens depending on complexity.

**Solution:** Budget-constrained reasoning where the model is given a compute budget (max reasoning tokens) and must produce its best answer within that budget. RL-trained models that learn to allocate reasoning effort efficiently.

### 6.2 The Accuracy-Cost Frontier

**Challenge:** As optimization techniques are stacked, the cumulative quality degradation can become significant even if each individual technique has minimal impact.

**Solution:** End-to-end quality monitoring that tracks task-specific metrics (not just perplexity) across the full optimization stack. Automated rollback if quality drops below threshold.

---

## Glossary of Key Terms

| Term | Definition |
|---|---|
| **TTFT** | Time-to-First-Token — latency before first generated token |
| **TPOT** | Time Per Output Token — latency per subsequent token |
| **Goodput** | Throughput of requests meeting SLO requirements |
| **Pareto Frontier** | Set of configurations where no metric can improve without degrading another |
| **Quantization** | Reducing numerical precision of model weights and activations |
| **Continuous Batching** | Dynamically adding/removing requests from inference batches |
| **Semantic Caching** | Caching responses for semantically similar queries |
| **Request Routing** | Directing queries to appropriately-sized models based on complexity |
| **LLMflation** | Rapid decline in the cost of equivalent-quality inference |

---

## Conclusion

LLM inference cost-latency optimization is a multi-layered discipline where stacking techniques across model, system, and application levels can reduce costs by 93-99% while simultaneously improving latency. The key insight is that optimization is not a single technique but a systematic stack where each layer compounds the savings of the layers below.

The immediate priority for any production deployment: right-size the model, quantize to FP8, deploy on an efficient serving engine, and enable KV cache optimization. These four steps alone typically achieve 85-95% cost reduction with minimal quality impact. Advanced techniques (routing, caching, speculative decoding) provide additional optimization for high-volume or latency-sensitive workloads.

---

## References

[1] NVIDIA. "Optimizing LLM Inference: From Goodput to SLOs." https://developer.nvidia.com

[2] BentoML. "LLM Inference Cost Optimization Guide." https://docs.bentoml.com

[3] Kwon, W., et al. (2023). "Efficient Memory Management for Large Language Model Serving with PagedAttention." *SOSP 2023*.

[4] Li, Y., et al. (2024). "EAGLE: Speculative Sampling Requires Rethinking Feature Uncertainty." *ICML 2024*.

[5] a16z. (2025). "The LLMflation Report: AI Inference Cost Trends."
