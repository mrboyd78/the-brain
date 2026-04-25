# LLM Memory Paging Systems and Hierarchical Memory Architectures

## Introduction

The fixed-size context window of transformer-based LLMs creates a fundamental constraint analogous to a computer with only RAM and no secondary storage — the system can only work with information that fits in its immediate memory, with everything else effectively nonexistent. Memory paging systems for LLMs address this limitation by borrowing directly from operating system memory management principles, creating hierarchical memory architectures that swap information between the model's active context (analogous to RAM), structured persistent storage (analogous to disk), and compressed representations (analogous to cache). This report provides a comprehensive analysis of LLM memory paging architectures, from infrastructure-level innovations like PagedAttention to agent-level memory management frameworks like MemGPT/Letta, examining their design principles, implementation mechanisms, performance characteristics, and production deployment patterns.

---

## 1. Theoretical Foundations and Historical Context

### 1.1 The OS Memory Management Analogy

Modern operating systems manage a memory hierarchy spanning CPU registers, L1/L2/L3 caches, DRAM, SSD/NVMe, and network-attached storage. Each tier offers different capacity-latency trade-offs, and the OS's virtual memory system transparently manages data movement between tiers. The key OS mechanisms that have been adapted for LLM memory management are:

- **Virtual Address Spaces:** Each process sees a continuous, private address space regardless of physical memory layout. PagedAttention's block tables provide analogous indirection for KV cache memory.
- **Demand Paging:** Pages are loaded from disk to RAM only when accessed (page fault). MemGPT's archival memory search operates similarly — retrieving information only when the agent determines it needs it.
- **Page Replacement Policies:** When RAM is full, the OS selects pages to evict (LRU, Clock, etc.). KV cache eviction policies (H2O, Scissorhands) serve the same function.
- **Memory-Mapped Files:** Files are transparently accessible through virtual memory addresses. Vector database-backed archival storage provides analogous transparent access to large knowledge bases.

### 1.2 The Three-Tier LLM Memory Model

The emerging standard architecture for LLM memory systems comprises three tiers:

**Tier 1 — Working Context (Hot):** The model's active context window. This is the only memory the model can directly attend to during inference. All information that influences generation must ultimately pass through this tier. Capacity: 4K-2M tokens depending on model architecture.

**Tier 2 — Structured State (Warm):** Persistent data structures maintained across inference calls but not directly in the context window. Includes core memory (user profiles, session state), conversation indices, and entity stores. Accessed via explicit retrieval operations. Capacity: megabytes to gigabytes.

**Tier 3 — Archival Store (Cold):** Large-scale, persistent storage for historical conversations, documents, knowledge bases, and accumulated experience. Accessed via semantic search (vector similarity), keyword search, or structured queries. Capacity: effectively unlimited (terabytes+).

### 1.3 Page Fault Mechanics in LLM Systems

In OS terminology, a "page fault" occurs when the processor accesses a virtual address that is not currently mapped to physical memory. In LLM memory systems, the analogous event occurs when the model needs information not present in its working context. Two types of page faults exist:

- **Explicit Page Faults:** The agent explicitly requests information via a tool call (e.g., `search_memory("user's delivery address")`). This is the dominant pattern in current systems.
- **Implicit Page Faults:** The system detects that the model's generation quality is degrading (e.g., hedging, saying "I don't recall") and automatically triggers a memory retrieval. This remains an active research area.

---

## 2. State-of-the-Art Review and Leading Implementations

### 2.1 PagedAttention: Infrastructure-Level Memory Paging

PagedAttention, implemented in vLLM, applies OS-style paging to the KV cache at the serving infrastructure level:

**Physical Block Management:** GPU HBM is divided into fixed-size blocks (pages) of KV cache storage. Each block stores K,V pairs for a configurable number of tokens. A global block allocator manages the free block pool.

**Block Tables:** Each request maintains a block table that maps logical token positions to physical block addresses, providing virtual-to-physical address translation analogous to an OS page table.

**On-Demand Allocation:** Blocks are allocated from the free pool only as tokens are generated, eliminating the pre-allocation waste of static systems. When a request completes, its blocks are returned to the free pool.

**Copy-on-Write (CoW):** When multiple requests share a common prefix, the shared KV blocks are physically shared via reference counting. When a request diverges from the shared prefix, only the divergent blocks are copied — reducing memory usage by 50-75% for workloads with common system prompts.

**Swap and Preemption:** When GPU memory is saturated, the least-recently-used request's blocks can be asynchronously swapped to CPU DRAM, with automatic swap-back when GPU memory becomes available. This enables graceful oversubscription rather than hard request rejection.

### 2.2 MemGPT/Letta: Agent-Level Memory Paging

MemGPT (now the Letta framework) applies OS memory management at the agent application level:

**Memory Architecture:**
- **Main Context:** The LLM's context window, divided into reserved sections: system instructions (~500 tokens), core memory (~2000 tokens), conversation buffer (~remaining tokens), and function call results.
- **Core Memory:** A persistent, structured store of essential information that is always present in the context window — user profile, agent persona, task-critical facts. Modified via explicit `core_memory_append` and `core_memory_replace` operations.
- **Recall Storage:** An indexed database of complete conversation history, searchable by content, timestamp, or metadata. Conversations are stored verbatim and retrievable via `conversation_search`.
- **Archival Storage:** A vector-indexed store for long-term knowledge, documents, and accumulated experience. Supports semantic search via `archival_memory_search` and insertion via `archival_memory_insert`.

**Self-Directed Memory Management:** The critical innovation is that the LLM itself decides when to read from or write to each memory tier, using function calls as memory operations. The model learns to proactively save important information before it would be truncated from the context window, and to search for relevant context when it recognizes knowledge gaps.

**Heartbeat Mechanism:** MemGPT uses a "heartbeat" system where the agent can send itself messages to trigger additional processing rounds without waiting for user input. This enables multi-step memory operations: search archival storage, retrieve relevant documents, update core memory, and compose a response — all within a single user turn.

### 2.3 Hierarchical Context Management (HCM) Systems

Emerging HCM systems formalize the multi-level buffer architecture:

**Buffer Architecture:**
- **L0 — Stable Task Semantics:** Unchanging task instructions and system prompts. Loaded once and pinned in context.
- **L1 — Condensed Long-Term Memory:** Periodically refreshed summaries of session history, key decisions, and accumulated knowledge. Updated every N turns.
- **L2 — High-Fidelity Short-Term Buffer:** Recent conversation turns at full resolution. Managed via sliding window with configurable depth.
- **L3 — Tool Results Buffer:** Transient buffer for function call results, search outputs, and retrieved documents. Cleared after consumption.

**Buffer Management Policies:**
- **Eviction:** When total buffer size exceeds context budget, content is evicted from L2 (oldest turns) and L3 (consumed results) first, with L0 and L1 protected.
- **Promotion:** Information deemed important during L2 processing is promoted to L1 (summarized and added to long-term memory).
- **Demotion:** Stale or low-utility L1 entries are demoted to archival storage.

### 2.4 Cascading KV Cache and Multi-Tier GPU Memory

At the hardware level, emerging systems implement memory paging across the GPU memory hierarchy:

**GPU HBM → CPU DRAM:** KV cache blocks for paused or preempted requests are swapped to CPU memory via PCIe/NVLink. vLLM's swap mechanism implements this transparently.

**CPU DRAM → NVMe SSD:** For very large numbers of concurrent conversations, even CPU DRAM may be insufficient. Systems like FlexGen implement KV cache offloading to NVMe storage, with prefetching algorithms that load blocks back to DRAM before they are needed.

**Prefix Caching Across Tiers:** Common prefixes (system prompts, few-shot examples) can be cached at each tier with automatic promotion/demotion based on access frequency. RadixAttention (SGLang) uses a radix tree to manage prefix sharing with LRU-based eviction.

---

## 3. Practical Implementations, Challenges, and Case Studies

### 3.1 Memory Tier Comparison

| Tier | Latency | Capacity | Content Type | Access Pattern | Implementation |
|---|---|---|---|---|---|
| **Working Context** | ~0ms (in-attention) | 4K-2M tokens | Active reasoning content | Continuous attention | LLM context window |
| **Core Memory** | ~0ms (in-context, pinned) | ~2K tokens | User profile, persona, critical facts | Always-available | MemGPT core_memory |
| **KV Cache (GPU)** | ~10μs (GPU memory access) | 10-100GB | Computed attention states | Per-token generation | PagedAttention blocks |
| **KV Cache (CPU)** | ~1ms (PCIe transfer) | 100GB-1TB | Swapped KV states | Preemption/resume | vLLM swap mechanism |
| **Recall Storage** | ~10ms (DB query) | Unlimited | Conversation history | Explicit search | PostgreSQL, SQLite |
| **Archival Storage** | ~50ms (vector search) | Unlimited | Knowledge, documents | Semantic search | Vector DB (Chroma, etc.) |
| **NVMe Storage** | ~100μs (SSD read) | Multi-TB | Cold KV cache, large archives | Batch prefetch | FlexGen-style offloading |

### 3.2 Production Architecture: Multi-Session Personal Assistant

A production personal assistant combines all paging tiers:

1. **Session Start:** Load core memory (user profile, preferences) into context. Initialize conversation buffer. KV cache allocated via PagedAttention blocks.
2. **During Session:** User messages and agent responses fill the conversation buffer. When buffer approaches capacity, oldest turns are saved to recall storage and summarized into L1 memory.
3. **Information Retrieval:** When the agent detects it needs historical information, it issues archival_memory_search or conversation_search, loading results into the transient tool results buffer.
4. **Session End:** Conversation saved to recall storage. Core memory updated with new user information. KV cache blocks freed.
5. **Session Resume:** Core memory loaded. Smart prefill retrieves the most recent conversation context from recall storage. KV cache rebuilt or restored from CPU cache.

### 3.3 Challenge: Page Fault Latency

**Problem:** When the agent performs a memory retrieval (page fault), the latency includes vector database query time (50-200ms), result formatting, and a new LLM forward pass to incorporate the retrieved context. In multi-step retrieval (searching, then reading, then synthesizing), total latency can reach 1-5 seconds.

**Mitigation Strategies:**
- **Predictive Prefetching:** Anticipate information needs based on conversation trajectory and pre-load relevant archival content.
- **Parallel Retrieval:** Issue multiple memory searches simultaneously when multiple knowledge gaps are detected.
- **Cached Search Results:** Maintain a small cache of recent search results to avoid redundant queries.

---

## 4. Rigorous Comparative Analysis

| System | Paging Level | Memory Tiers | Self-Management | Scalability | Production Readiness |
|---|---|---|---|---|---|
| **PagedAttention (vLLM)** | Infrastructure (KV cache) | GPU HBM ↔ CPU DRAM | No (system-managed) | Excellent | Production standard |
| **MemGPT/Letta** | Agent (context content) | Context ↔ Recall ↔ Archival | Yes (LLM-directed) | Good | Maturing |
| **HCM (Multi-Buffer)** | Agent (structured buffers) | L0/L1/L2/L3 buffers | Hybrid (policy + LLM) | Good | Research/early production |
| **FlexGen** | Infrastructure (KV offloading) | GPU ↔ CPU ↔ NVMe | No (throughput-optimized) | Limited (single-GPU focus) | Research |
| **RadixAttention (SGLang)** | Infrastructure (prefix sharing) | Prefix tree in GPU memory | No (automatic LRU) | Excellent | Production |

---

## 5. Identified Challenges, Proposed Solutions, and Future Research Avenues

### 5.1 The Self-Management Quality Problem

**Challenge:** When the LLM manages its own memory, the quality of memory operations depends on the model's intelligence. Weaker models may forget to save important information, search with poor queries, or overwrite critical core memory entries.

**Solution:** Hybrid management combining LLM-directed operations with rule-based policies: automatic summarization of old conversation turns (rule-based), automatic core memory updates for detected entities (rule-based), with the LLM providing higher-level curation decisions. Safety rails that prevent deletion of protected memory entries.

### 5.2 Cross-Session Context Continuity

**Challenge:** Resuming a conversation after hours or days requires reconstructing relevant context from cold storage. Simply loading the most recent summary may miss critical information from earlier sessions.

**Solution:** Session-boundary checkpoints that capture a complete snapshot of the agent's state (core memory, active summaries, key entities, open tasks). Smart session resumption that loads the checkpoint and performs targeted retrieval based on the user's opening message.

### 5.3 Memory Consistency in Multi-Agent Systems

**Challenge:** When multiple agents share access to the same memory tiers (e.g., a customer support team), concurrent read/write operations can create race conditions — one agent updates a user profile while another reads stale data.

**Solution:** Transactional memory operations with optimistic concurrency control. Each agent reads a version-stamped snapshot; writes are verified against the current version before committing. Conflict resolution policies prioritize more recent or more authoritative updates.

---

## 6. Emerging Trends, Future Directions, and Broader Impact

### 6.1 Unified Memory Abstraction Layer

Future systems will provide a single memory API that abstracts over all physical tiers — the agent simply reads and writes to "memory" without knowing whether the data resides in the context window, GPU HBM, CPU DRAM, vector database, or cold storage. The memory management system handles placement, promotion, demotion, and eviction transparently.

### 6.2 Hardware-Software Co-Design

As LLM memory hierarchies become standard, hardware vendors will provide explicit support: CXL-attached memory pools for KV cache expansion, dedicated DMA engines for KV cache migration, and persistent memory (Intel Optane successor technologies) for non-volatile session state.

### 6.3 Learned Memory Management Policies

RL-trained memory managers that learn optimal paging, eviction, and prefetching policies from workload traces. These learned policies can adapt to application-specific access patterns, outperforming hand-designed heuristics by 20-40% on throughput metrics.

---

## Glossary of Key Terms

| Term | Definition |
|---|---|
| **PagedAttention** | OS-style virtual memory paging for KV cache management |
| **Block Table** | Per-request mapping from logical token positions to physical memory blocks |
| **MemGPT** | OS-inspired agent memory framework with self-directed memory management |
| **Core Memory** | Persistent, always-in-context store for critical agent/user information |
| **Archival Storage** | Vector-indexed long-term storage for documents and knowledge |
| **Recall Storage** | Indexed database of conversation history |
| **Copy-on-Write** | Memory sharing technique where shared blocks are copied only when modified |
| **Demand Paging** | Loading data from storage to active memory only when accessed |
| **Page Fault** | Event triggered when needed data is not in active memory tier |
| **HCM** | Hierarchical Context Management — multi-buffer memory architecture |
| **RadixAttention** | Prefix-sharing KV cache management using radix tree data structure |
| **CXL** | Compute Express Link — interconnect for memory pool expansion |

---

## Conclusion

LLM memory paging systems represent the maturation of LLM serving from stateless, single-inference systems to stateful, multi-session platforms. The field operates at two complementary levels: infrastructure-level paging (PagedAttention, RadixAttention) that optimizes physical memory utilization for the KV cache, and agent-level paging (MemGPT, HCM) that manages the semantic content within the context window.

For production deployment:
- **Infrastructure tier:** PagedAttention is non-negotiable for any production serving system. RadixAttention or equivalent prefix caching adds 2-4x efficiency for workloads with shared prefixes.
- **Agent tier:** MemGPT-style architectures with core memory, recall storage, and archival storage provide the foundation for persistent, multi-session agents.
- **Hybrid approach:** Combining infrastructure-level KV cache management with agent-level content management through structured buffer hierarchies delivers the best overall system performance.

The convergence of these tiers toward a unified memory abstraction layer — where agents interact with a seamless memory continuum from context window to cold storage — represents the next major architectural evolution in LLM systems.

---

## References

[1] Kwon, W., et al. (2023). "Efficient Memory Management for Large Language Model Serving with PagedAttention." *SOSP 2023*. https://arxiv.org/abs/2309.06180

[2] Packer, C., et al. (2023). "MemGPT: Towards LLMs as Operating Systems." https://arxiv.org/abs/2310.08560

[3] Zheng, L., et al. (2024). "SGLang: Efficient Execution of Structured Language Model Programs." https://arxiv.org/abs/2312.07104

[4] Sheng, Y., et al. (2023). "FlexGen: High-Throughput Generative Inference of Large Language Models with a Single GPU." *ICML 2023*. https://arxiv.org/abs/2303.06865

[5] Letta AI. "Letta: The Open-Source Framework for Stateful LLM Agents." https://www.letta.com

[6] Various (2024-2025). "Hierarchical Context Management for Multi-Session LLM Agents."
