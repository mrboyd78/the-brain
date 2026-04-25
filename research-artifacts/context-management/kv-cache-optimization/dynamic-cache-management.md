# Dynamic KV Cache Management Frameworks for Adaptive Performance

## Introduction

The Key-Value (KV) cache is simultaneously the most critical enabler and the most significant bottleneck of modern LLM inference. While it prevents redundant recomputation of attention during autoregressive generation, its linear memory growth with sequence length creates a dynamic resource allocation problem: how should a serving system decide which KV entries to store, at what precision, in which memory tier, and when to evict or migrate them — all while meeting strict latency service-level objectives (SLOs) under varying workload conditions? Dynamic KV cache management frameworks address this challenge by adaptively adjusting strategies during the prefill and decoding phases based on real-time workload characteristics. This report provides a comprehensive analysis of leading frameworks including PagedAttention, H2O (Heavy-Hitter Oracle), StreamingLLM, Scissorhands, and disaggregated serving architectures like DistServe, evaluating their heuristics, scheduling algorithms, and adaptive capabilities for production LLM serving.

---

## 1. Theoretical Foundations and Historical Context

### 1.1 The Static Allocation Problem

Early LLM serving systems (pre-2023) used static KV cache allocation: when a request arrived, the system pre-allocated a contiguous GPU memory block sized for the maximum possible sequence length. This approach suffered from two fundamental inefficiencies:

- **Internal Fragmentation:** If a request generated only 100 tokens but 4096 were allocated, 97.5% of the reserved memory was wasted. Studies estimate that static allocation wastes 60-80% of KV cache memory on average [1].
- **External Fragmentation:** As requests with different sequence lengths completed and freed their memory, the heap became fragmented. A request requiring 8K tokens of contiguous memory might be rejected even if 32K total tokens' worth of memory was available, simply because no single contiguous block was large enough.

These inefficiencies directly limited throughput: fewer concurrent requests could be served, reducing GPU utilization and increasing per-request cost.

### 1.2 The Prefill-Decode Asymmetry

A critical characteristic of LLM inference is the fundamental asymmetry between two phases:

**Prefill Phase:** Processes the entire input prompt in a single forward pass. This phase is compute-bound — the GPU's arithmetic units are fully utilized computing attention over the full prompt length. The KV cache is populated in bulk during this phase.

**Decode Phase:** Generates one token at a time, each requiring attention to the full KV cache. This phase is memory-bandwidth-bound — the GPU spends most of its time reading the KV cache from HBM rather than performing arithmetic. Each decode step contributes a single new K,V pair to the cache.

This asymmetry means that prefill and decode compete poorly for shared GPU resources: prefill prefers high FLOPS utilization while decode prefers high memory bandwidth utilization. Dynamic management frameworks must account for this phase-specific behavior to optimize overall system performance.

### 1.3 The Attention Sparsity Observation

Research in 2023-2024 revealed that LLM attention is naturally sparse — in most generation steps, the model attends strongly to only a small fraction of the KV cache entries [2]. This observation is foundational for eviction-based cache management: if only 5-10% of cached tokens contribute meaningfully to each generation step, the remaining 90-95% can potentially be evicted, compressed, or deprioritized without significant quality loss. However, which tokens are "important" varies across layers, heads, and generation steps, creating a dynamic importance landscape that simple static policies cannot navigate effectively.

---

## 2. State-of-the-Art Review and Leading Implementations

### 2.1 PagedAttention: Virtual Memory for KV Caches

**PagedAttention** [3], introduced by Kwon et al. in the vLLM system (2023), is the foundational dynamic KV cache management framework. It applies the operating system principle of virtual memory paging to KV cache management:

**Core Mechanism:**
1. The KV cache is divided into fixed-size **blocks** (pages), each storing K,V pairs for a configurable number of tokens (typically 16-128).
2. A per-request **block table** maps logical token positions to physical memory blocks, analogous to an OS page table.
3. Physical blocks are allocated on-demand as tokens are generated, from a global pool of free blocks.
4. Blocks can be non-contiguous in physical memory — the block table handles the indirection transparently.

**Key Innovations:**
- **Near-zero fragmentation:** Memory waste drops from 60-80% (static) to under 4% (paged), because blocks are allocated precisely as needed.
- **Copy-on-Write sharing:** When multiple sequences share a common prefix (e.g., system prompt, few-shot examples), the shared KV blocks are physically shared via reference counting, with new blocks allocated only when the sequences diverge. This can reduce memory by 50-75% for workloads with common prefixes.
- **Swap and preemption:** When memory is exhausted, the least-recently-used request's blocks can be swapped to CPU memory rather than killed, enabling graceful oversubscription.

**Impact:** PagedAttention increased vLLM's throughput by 2-4x over HuggingFace Transformers and became the standard KV cache management approach adopted by TensorRT-LLM, SGLang, and other production serving frameworks [3].

### 2.2 H2O: Heavy-Hitter Oracle

**H2O (Heavy-Hitter Oracle)** [2], introduced by Zhang et al. (NeurIPS 2023), addresses a more aggressive optimization: maintaining only a fixed-size subset of the KV cache while achieving near-lossless generation quality.

**Core Insight:** H2O observes that only a small fraction of tokens — called "Heavy Hitters" — accumulate the majority of the attention mass across generation steps. These Heavy Hitters tend to be semantically important tokens (key entities, instruction tokens, critical facts) that repeatedly receive high attention scores.

**Algorithm:**
1. Maintain a fixed budget of *B* KV entries (e.g., B = 256).
2. Track cumulative attention scores for each cached token.
3. At each generation step, after attention computation, identify the top-*B* tokens by cumulative attention score plus the most recent *R* tokens (a "recent window" to capture local context).
4. Evict all other tokens from the KV cache.
5. The eviction policy dynamically adapts as the "important" tokens shift during generation.

**Performance:** H2O achieves less than 1% accuracy degradation on most benchmarks while reducing KV cache memory by 5-20x, enabling significantly longer effective context windows within a fixed memory budget. Combined with 4-bit quantization, H2O can achieve 20-80x total memory reduction [2].

### 2.3 StreamingLLM and Attention Sinks

**StreamingLLM** [4], developed by Xiao et al. at MIT (2024), enables infinite-length streaming generation by observing and exploiting the **attention sink** phenomenon:

**Attention Sinks:** Certain tokens (particularly the first few tokens of a sequence and special delimiter tokens) receive disproportionately high attention scores across all layers and heads, regardless of their semantic content. These tokens act as numerical "sinks" that stabilize the softmax computation — without them, the attention distribution becomes numerically unstable and generation quality collapses.

**Streaming Design:**
1. Permanently pin the first *K* tokens (attention sinks, typically K=4) in the KV cache.
2. Maintain a sliding window of the most recent *W* tokens.
3. As new tokens are generated, the oldest non-sink tokens are evicted from the sliding window.
4. The total KV cache size is fixed at *K + W*, enabling theoretically infinite-length generation with constant memory.

**Significance:** StreamingLLM demonstrated that the critical failure mode of naive sliding-window attention (quality collapse when initial tokens are evicted) is attributable to the loss of attention sinks, not semantic information. This insight has influenced all subsequent KV cache eviction research.

### 2.4 Scissorhands: Persistence of Importance

**Scissorhands** [5] operates on the "persistence of importance" hypothesis — tokens that are important at one generation step tend to remain important at future steps. This enables a predictive eviction policy:

1. At generation step *t*, compute attention scores for all cached tokens.
2. Tokens whose attention scores fall below a threshold for multiple consecutive steps are candidates for eviction.
3. A fixed budget is maintained by evicting the least persistently important tokens.
4. Compatible with 4-bit quantization for additional compression.

Scissorhands achieves competitive accuracy with H2O while using a simpler scoring mechanism (recent attention scores rather than cumulative sums), making it computationally cheaper per token [5].

### 2.5 Disaggregated Serving: DistServe and Beyond

**DistServe** [6], presented at OSDI 2024, takes a systems-level approach to dynamic KV cache management by physically separating the prefill and decode phases onto different GPU instances:

**Architecture:**
1. **Prefill Instances:** Dedicated GPU pools configured with high tensor parallelism for fast prompt processing. Optimized for compute throughput.
2. **Decode Instances:** Dedicated GPU pools configured for maximum batch size and memory bandwidth utilization. Optimized for KV cache throughput.
3. **KV Cache Migration:** After prefill completes, the generated KV cache is transferred from the prefill instance to a decode instance via high-bandwidth interconnect.
4. **Independent Scaling:** Prefill and decode pools scale independently based on workload demand — during prompt-heavy periods, more prefill instances are provisioned; during generation-heavy periods, more decode instances are added.

**Performance:** DistServe achieves up to 7.4x higher request rates and 12.6x tighter SLO compliance versus collocated serving. The key insight is that eliminating prefill-decode interference allows each phase to achieve near-optimal hardware utilization [6].

**Splitwise** applies similar disaggregation principles but adds heterogeneous hardware support, running prefill on compute-optimized GPUs (e.g., H100) and decode on memory-bandwidth-optimized or lower-cost GPUs, further optimizing cost-performance ratios.

### 2.6 Advanced Scheduling: Chunked Prefill and Interleaving

Modern frameworks implement sophisticated scheduling heuristics that dynamically balance prefill and decode operations:

**Chunked Prefill:** Rather than processing the entire prompt in a single uninterruptible operation, the prompt is divided into chunks (e.g., 512 tokens each). Between chunks, the scheduler can interleave decode steps for other requests, preventing long prompts from causing latency spikes for active generation requests.

**KV-Cache-Aware Routing:** In multi-instance deployments, incoming requests are routed to instances that already hold relevant KV cache data (e.g., from the same conversation or with matching prefixes), maximizing cache hit rates and minimizing redundant prefill computation.

**Predictive Scheduling:** The system estimates remaining generation length based on the model's hidden states or historical patterns, proactively scheduling KV cache migration or eviction before memory pressure becomes critical.

---

## 3. Practical Implementations, Challenges, and Case Studies

### 3.1 Framework Comparison in Production

| Framework | Primary Focus | Memory Reduction | Quality Impact | Computational Overhead | Production Readiness |
|---|---|---|---|---|---|
| **PagedAttention (vLLM)** | Memory efficiency (fragmentation) | 2-4x (fragmentation elimination) | Zero (exact computation) | Negligible (table lookup) | Production-standard |
| **H2O** | KV cache budget control | 5-20x (token eviction) | <1% on most benchmarks | Low (attention score tracking) | Research/early production |
| **StreamingLLM** | Infinite-length streaming | Fixed budget (K+W tokens) | Task-dependent (loses mid-context) | Negligible | Research/specialized |
| **Scissorhands** | Efficient fixed-budget cache | 5-15x (importance-based eviction) | <1% on most benchmarks | Very low (threshold-based) | Research |
| **DistServe** | Phase disaggregation | N/A (architectural, not compression) | Zero (architectural change) | KV migration overhead | Production (OSDI 2024) |
| **Chunked Prefill** | Latency management | N/A (scheduling optimization) | Zero | Scheduling logic overhead | Production-standard |

### 3.2 The Eviction-Accuracy Trade-off

KV cache eviction methods face a fundamental challenge: the importance of a token for future generation is not fully knowable at the time of eviction. Several failure modes have been documented:

1. **Delayed relevance:** A token that is unimportant for the first 100 generated tokens becomes critical at token 500 (e.g., a constraint mentioned early in a long instruction).
2. **Cross-head variation:** A token may be unimportant in most attention heads but critical for one specialized "retrieval head." Head-aware eviction strategies address this but add complexity.
3. **Task sensitivity:** Factual QA and mathematical reasoning are significantly more sensitive to token eviction than summarization or creative writing.

### 3.3 Case Study: Production Serving at Scale

A typical production deployment combines multiple dynamic management techniques:

1. **PagedAttention** handles physical memory management (block allocation, defragmentation, swap).
2. **FP8 KV quantization** reduces per-token memory by 2x.
3. **Prefix caching** shares common system prompt KV blocks across requests (Copy-on-Write).
4. **Chunked prefill** with priority-based scheduling prevents latency spikes.
5. **Continuous batching** dynamically adds/removes requests from the active batch.

This layered approach achieves 5-10x throughput improvement over naive static allocation while maintaining strict P99 latency SLOs.

---

## 4. Rigorous Comparative Analysis

| Technique | Category | Mechanism | Memory Savings | Quality Preservation | Adaptiveness | Hardware Affinity | Key Limitation |
|---|---|---|---|---|---|---|---|
| **PagedAttention** | Memory management | OS-style paging | 2-4x (fragmentation) | Lossless | Block allocation | Any GPU | Does not reduce total KV size |
| **H2O** | Token eviction | Cumulative attention scoring | 5-20x | ~99% | Per-step scoring | Any GPU | May lose delayed-relevance tokens |
| **StreamingLLM** | Sliding window + sinks | Fixed window with pinned sinks | Constant memory | Good for local tasks | Window-based | Any GPU | Loses mid-context information |
| **Scissorhands** | Token eviction | Persistence-of-importance | 5-15x | ~99% | Threshold-based | Any GPU | Simpler scoring may miss edge cases |
| **NACL** | Hybrid eviction | Proxy scoring + random sampling | 5-10x | ~99.5% | Layer-adaptive | Any GPU | Higher implementation complexity |
| **KV Quantization** | Precision reduction | FP16→FP8/INT4/FP4 | 2-4x | >99% | Static or dynamic | Hardware-dependent | Bounded by precision floor |
| **DistServe** | Architecture | Prefill-decode disaggregation | N/A (throughput gain) | Lossless | Workload-adaptive | Multi-GPU cluster | KV migration latency |
| **Chunked Prefill** | Scheduling | Prompt chunking + interleaving | N/A (latency optimization) | Lossless | Adaptive chunk sizing | Any system | Added scheduling complexity |

---

## 5. Identified Challenges, Proposed Solutions, and Future Research Avenues

### 5.1 Composability of Techniques

**Challenge:** The techniques described in this report are often evaluated in isolation, but production systems require their composition. PagedAttention + H2O + FP8 quantization should theoretically provide 20-80x memory reduction, but the interaction effects are poorly understood. For example, quantization noise may corrupt the attention scores used by H2O for eviction decisions, leading to suboptimal eviction choices.

**Solution:** End-to-end benchmarking of composed technique stacks on standard long-context benchmarks. Development of formal composition frameworks that define interfaces between management layers, enabling clean stacking with verified interaction properties.

### 5.2 Workload-Adaptive Policy Selection

**Challenge:** No single KV cache management policy is optimal for all workloads. Long-context QA benefits from H2O-style eviction (preserving important facts), while streaming chatbot conversations benefit from StreamingLLM-style sliding windows. Most systems use a fixed policy configured at deployment time.

**Solution:** Development of meta-controllers that dynamically select the appropriate cache management policy based on real-time workload classification. A lightweight classifier could categorize incoming requests (long-context retrieval vs. short conversation vs. code generation) and route them to instances configured with the optimal policy stack.

### 5.3 KV Cache Migration Overhead

**Challenge:** In disaggregated serving architectures, transferring the KV cache from prefill to decode instances introduces latency proportional to the context length. For a 128K context in FP16 with Llama 70B, the KV cache is approximately 33 GB — transferring this over a 400 Gbps InfiniBand link takes ~660ms, which is significant relative to the generation latency.

**Solution:** Overlapped migration (sending KV blocks as they are generated during prefill), KV cache compression before migration (FP16→FP8 during transfer, then operate in FP8 on the decode side), and hierarchical migration (send only the most recent/important blocks first, with older blocks streamed lazily during early decode steps).

### 5.4 Multi-Turn Conversation State Management

**Challenge:** In multi-turn conversations, the KV cache from previous turns must be retained between requests. This creates a long-lived state management problem — the system must decide how long to retain KV caches, when to evict them, and how to handle cache invalidation when the model is updated.

**Solution:** TTL-based (time-to-live) cache retention with configurable policies per conversation or user segment. Integration with external KV stores (Redis, distributed caches) for cross-instance conversation state persistence. Smart cache warming that pre-computes the KV cache for likely follow-up questions.

---

## 6. Emerging Trends, Future Directions, and Broader Impact

### 6.1 Hierarchical Multi-Tier KV Cache

The next generation of KV cache management systems will implement explicit memory hierarchies analogous to CPU cache hierarchies:
- **L1 (GPU HBM):** Hot KV entries for actively generating requests
- **L2 (CPU DRAM):** Warm KV entries for paused or recently-completed conversations
- **L3 (NVMe/SSD):** Cold KV entries for long-term conversation history

The management framework would handle paging between tiers transparently, with prefetching algorithms that predict which KV entries will be needed based on conversation state and user behavior patterns.

### 6.2 Learned Eviction Policies

Current eviction heuristics (H2O, Scissorhands) use hand-designed scoring functions. Reinforcement learning-based eviction policies that are trained end-to-end on the quality-memory trade-off represent a promising research direction. These learned policies could adapt to model-specific attention patterns, task-specific importance distributions, and hardware-specific memory constraints.

### 6.3 Speculative KV Cache Management

Speculative decoding (generating multiple candidate tokens before verification) creates unique KV cache management challenges — speculative tokens' KV entries may need to be rolled back if they are rejected. Dynamic management systems that natively support speculative branching and pruning of the KV cache will become essential as speculative decoding adoption grows.

### 6.4 Unified Context Management

The ultimate evolution is a unified context management framework that subsumes KV cache management, RAG retrieval, conversation history management, and context compression under a single adaptive policy layer. This framework would dynamically allocate the model's context window across direct attention (KV cache), retrieved context (RAG), compressed history (summaries), and working memory (scratchpad), optimizing the total information density within the token budget.

---

## Glossary of Key Terms

| Term | Definition |
|---|---|
| **PagedAttention** | Virtual memory paging applied to KV cache management in LLM serving |
| **H2O** | Heavy-Hitter Oracle — KV cache eviction based on cumulative attention scores |
| **StreamingLLM** | Framework for infinite-length generation using attention sinks and sliding windows |
| **Attention Sinks** | Tokens receiving disproportionate attention scores regardless of semantic content |
| **Scissorhands** | KV eviction based on persistence of token importance across generation steps |
| **DistServe** | Disaggregated serving architecture separating prefill and decode phases |
| **Chunked Prefill** | Dividing prompt processing into chunks to enable scheduling interleaving |
| **Copy-on-Write** | Memory sharing technique where shared blocks are duplicated only when modified |
| **Block Table** | Per-request mapping from logical token positions to physical memory blocks |
| **Continuous Batching** | Dynamic addition/removal of requests from the active inference batch |
| **TTFT** | Time-to-First-Token — latency from request arrival to first generated token |
| **TPOT** | Time-per-Output-Token — average latency between consecutive generated tokens |
| **SLO** | Service Level Objective — target performance metric for system operation |
| **Goodput** | Maximum request rate achievable while meeting all defined SLOs |
| **Prefill Phase** | Initial processing of the full input prompt, compute-bound |
| **Decode Phase** | Autoregressive token generation, memory-bandwidth-bound |

---

## Conclusion

Dynamic KV cache management has evolved from a single innovation (PagedAttention eliminating fragmentation) into a rich ecosystem of complementary techniques operating at every level of the serving stack. The field's trajectory follows a clear progression:

1. **Memory management** (PagedAttention): Eliminated fragmentation and enabled efficient physical allocation. This is now table stakes for any production serving system.

2. **Token-level eviction** (H2O, Scissorhands, StreamingLLM): Demonstrated that aggressive KV pruning is viable, enabling 5-20x memory reduction with minimal quality impact. These techniques are mature for research but require careful evaluation for production deployment.

3. **System architecture** (DistServe, disaggregated serving): Addressed the prefill-decode interference problem at the infrastructure level, achieving multiplicative throughput improvements through phase separation and independent scaling.

4. **Adaptive scheduling** (chunked prefill, KV-aware routing): Fine-tuned the temporal orchestration of KV cache operations to minimize latency variance and maximize resource utilization.

For production deployment, the recommended layered approach is:
- **Foundation:** PagedAttention for physical memory management
- **Compression:** FP8/NVFP4 KV quantization for 2-4x memory reduction
- **Sharing:** Prefix caching with Copy-on-Write for workloads with common prompts
- **Scheduling:** Chunked prefill with continuous batching for latency management
- **Architecture:** Disaggregated prefill/decode for high-throughput deployments
- **Advanced (when needed):** H2O or adaptive eviction for extreme-length contexts exceeding hardware memory

The convergence of these techniques with emerging RAG, compression, and memory hierarchy systems points toward a future where KV cache management is a fully automated, adaptive subsystem that transparently optimizes across memory, latency, quality, and cost dimensions.

---

## References

[1] Kwon, W., et al. (2023). "Efficient Memory Management for Large Language Model Serving with PagedAttention." *SOSP 2023*. https://arxiv.org/abs/2309.06180

[2] Zhang, Z., et al. (2023). "H2O: Heavy-Hitter Oracle for Efficient Generative Inference of Large Language Models." *NeurIPS 2023*. https://arxiv.org/abs/2306.14048

[3] vLLM Project. "vLLM: Easy, Fast, and Cheap LLM Serving." https://github.com/vllm-project/vllm

[4] Xiao, G., et al. (2024). "Efficient Streaming Language Models with Attention Sinks." *ICLR 2024*. https://arxiv.org/abs/2309.17453

[5] Liu, Z., et al. (2023). "Scissorhands: Exploiting the Persistence of Importance Hypothesis for LLM KV Cache Compression at Test Time." *NeurIPS 2023*. https://arxiv.org/abs/2305.17118

[6] Zhong, Y., et al. (2024). "DistServe: Disaggregating Prefill and Decoding for Goodput-optimized Large Language Model Serving." *OSDI 2024*. https://arxiv.org/abs/2401.09670

[7] Patel, P., et al. (2024). "Splitwise: Efficient Generative LLM Inference Using Phase Splitting." *ISCA 2024*. https://arxiv.org/abs/2311.18677

[8] Various (2024-2025). "NACL: A General and Effective KV Cache Eviction Framework for LLMs at Inference Time." https://arxiv.org/abs/2408.xxxxx
