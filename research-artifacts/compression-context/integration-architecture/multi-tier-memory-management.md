# Managing Compressed Context Across Multi-Tier Memory Systems: Strategies and Performance

## Introduction

Modern LLM inference systems operate across a complex memory hierarchy spanning at least four tiers: GPU High-Bandwidth Memory (HBM), CPU main memory (DRAM), local persistent storage (NVMe SSD), and remote/distributed storage (object stores, networked caches). As context windows grow beyond 100K tokens and serving systems manage thousands of concurrent sessions, the KV cache and associated context data can consume hundreds of gigabytes—far exceeding the capacity of any single GPU's HBM. This report examines best practices for managing zstd-compressed and OpenZL-compressed LLM context data across these tiers, analyzing storage and retrieval strategies, cache invalidation, data consistency, and the impact on end-to-end system performance and cost.

---

## 1. Theoretical Foundations: Memory Hierarchy and LLM Context

### 1.1 The Memory Tiering Problem

The fundamental challenge is an impedance mismatch between data capacity needs and access latency:

| Tier | Medium | Capacity (Typical) | Bandwidth | Latency | Cost ($/GB) |
|:---|:---|:---|:---|:---|:---|
| **T0: GPU HBM** | HBM3e | 80–192 GB per GPU | 3–5 TB/s | ~100 ns | $20–50 |
| **T1: CPU DRAM** | DDR5-5600 | 256 GB – 2 TB per socket | 80–120 GB/s | ~80 ns | $3–8 |
| **T2: Local NVMe** | NVMe Gen5 | 2–30 TB | 10–14 GB/s | ~5–20 μs | $0.10–0.30 |
| **T3: Remote/Object** | S3/GCS/Azure Blob | Unlimited | 1–10 GB/s (network) | ~5–50 ms | $0.02–0.05 |

An LLM serving system with 1,000 concurrent sessions, each with 128K tokens of context, generates approximately 320 GB of KV cache data (assuming FP16, 32 layers, 8K hidden dimension). This exceeds even the largest multi-GPU HBM configurations, necessitating tiered management.

### 1.2 Compression's Role in Tiered Memory

Compression serves two complementary functions in multi-tier memory management:

1. **Capacity amplification:** By compressing data 2–5×, each tier's effective capacity increases proportionally, allowing more sessions to remain at higher (faster) tiers.
2. **Bandwidth amplification:** Compressed data requires fewer bytes to transfer between tiers. If compression ratio exceeds the overhead of compression/decompression, net transfer throughput improves.

The net benefit depends on the **compression throughput ratio (CTR)**:

```
CTR = Compression_Ratio / (1 + Compression_CPU_Time × Data_Rate / Transfer_Bandwidth)
```

When CTR > 1, compression is beneficial; when CTR < 1, compression adds overhead without proportional benefit.

---

## 2. Tier-Specific Compression Strategies

### 2.1 Tier 0: GPU HBM

**Data type:** Active KV cache tensors (FP16/BF16), attention masks, position embeddings.

**Compression approach:** Quantization-based (not zstd/OpenZL).

GPU HBM is accessed at compute speeds (~3 TB/s) with nanosecond latency. Lossless byte-level compression is impractical here because:
- Decompression would require GPU compute cycles, competing with inference workloads.
- FP16 tensor data has high byte-level entropy, yielding poor lossless ratios (1.1–1.5×).
- Quantization (FP16 → INT8, INT4, or 2-bit) achieves 2–8× reduction with minimal quality loss and can be fused into the attention computation kernel.

| Technique | Ratio | Quality Loss | GPU Compute Overhead |
|:---|:---|:---|:---|
| FP16 → INT8 | 2× | Minimal | Very low (fused dequant) |
| FP16 → INT4 (KVQuant) | 4× | Low–moderate | Low |
| FP16 → 2-bit (TurboQuant) | 8× | Moderate | Moderate |
| Zstd on FP16 | 1.3–1.5× | None | High (CPU required) | 

**Recommendation:** Do not use zstd/OpenZL on T0 data. Use quantization for GPU-resident KV cache.

### 2.2 Tier 1: CPU DRAM

**Data type:** Offloaded KV cache blocks, active session context, hot conversation history, pre-compressed prompt templates.

**Compression approach:** Zstd level 1–3 with dictionary, or LZ4 for ultra-low-latency needs.

CPU DRAM serves as the primary overflow tier for KV cache data and the hot session context store. Compression at this tier must minimize both memory footprint and decompression latency:

| Data Type | Compression | Expected Ratio | Decompress Latency (10 KB) | Effective Capacity Gain |
|:---|:---|:---|:---|:---|
| KV cache blocks (quantized INT8) | LZ4 | 1.5–2.0× | ~2 μs | 50–100% more blocks |
| Conversation JSON | Zstd L1 + dict | 3.0–5.0× | ~5 μs | 200–400% more sessions |
| Tool responses | Zstd L1 + dict | 4.0–7.0× | ~3 μs | 300–600% more entries |
| Prompt templates | Zstd L3 (pre-compressed) | 4.0–6.0× | ~4 μs | 300–500% |

**Implementation pattern:**
```
[GPU Request] → Check T1 cache → [Hit] → Decompress (zstd/LZ4) → Transfer to GPU
                                → [Miss] → Promote from T2 → Compress → Store in T1
```

### 2.3 Tier 2: Local NVMe SSD

**Data type:** Warm session context, overflow KV cache, seekable conversation archives, RAG document cache.

**Compression approach:** Zstd level 3–7, seekable format for random access.

NVMe storage offers high capacity at ~100× lower cost than DRAM. Compression at this tier is highly effective because:
- I/O is the bottleneck, not CPU compute. Higher compression levels (3–7) reduce I/O volume without significantly impacting overall latency.
- The seekable format enables per-message or per-block random access without decompressing entire session files.

| Configuration | Read IOPS (4 KB) | Effective Read BW (Compressed) | Decompress BW | Net Throughput |
|:---|:---|:---|:---|:---|
| Uncompressed | 1M IOPS, 7 GB/s | 7 GB/s | N/A | 7 GB/s |
| Zstd L3 (3× ratio) | 1M IOPS, 7 GB/s compressed | 21 GB/s effective | 1.5 GB/s CPU | ~1.5 GB/s (CPU-bound) |
| Zstd L3 + io_uring async | 1M IOPS | 21 GB/s effective | 1.5 GB/s ×4 threads | ~5 GB/s (multi-threaded) |

**Key insight:** The net throughput is CPU-bound at decompression, not SSD-bound at I/O. Using 4–8 decompression threads in parallel with io_uring prefetch achieves near-wire-speed effective throughput.

### 2.4 Tier 3: Remote/Object Storage

**Data type:** Cold session archives, historical conversations, cross-region context replication, compliance-mandated retention.

**Compression approach:** Zstd level 10–19 (maximum ratio), OpenZL for structured archives.

At this tier, storage cost dominates and access latency is already high (5–50 ms network). Maximum compression is justified:

| Algorithm | Ratio (Session JSON) | Compress Time (1 MB) | Cost Savings at $0.023/GB/mo |
|:---|:---|:---|:---|
| Uncompressed | 1× | 0 | $0.023 |
| Zstd L3 | 3.5× | ~2 ms | $0.0066 (71% saving) |
| Zstd L10 | 4.5× | ~30 ms | $0.0051 (78% saving) |
| Zstd L19 | 5.0× | ~200 ms | $0.0046 (80% saving) |
| OpenZL (structured) | 7.0× | ~10 ms (trained Plan) | $0.0033 (86% saving) |

---

## 3. Data Movement and Tier Promotion/Demotion

### 3.1 Promotion Policy (Cold → Hot)

When context data is needed for inference but resides at a lower tier, it must be promoted:

```
T3 (Object Store) → T2 (NVMe) → T1 (DRAM) → T0 (GPU HBM)
```

At each promotion step:
1. **Fetch** compressed data from the source tier.
2. **Decompress** if crossing a compression boundary (T3→T2 may remain compressed at a different level; T1→T0 must be decompressed/dequantized).
3. **Re-compress** at the target tier's compression level if remaining compressed (e.g., re-compressing from zstd L19 to zstd L1 when moving from T2 to T1 for faster decompression on hot reads).

**Optimization: Avoid re-compression.** If data is already compressed at zstd L3 on T2, it can be cached in T1 at the same compression level without re-compressing. Zstd decompression speed is independent of the compression level used, so L19-compressed data decompresses at the same speed as L1-compressed data [1].

### 3.2 Demotion Policy (Hot → Cold)

When session activity ceases or memory pressure rises, context data is demoted:

```
T0 (GPU) → Quantize → T1 (DRAM) → Zstd L1 compress → T2 (NVMe) → Zstd L7+ recompress → T3 (Object)
```

**Asynchronous demotion:** Compression for demotion should be performed asynchronously to avoid blocking the inference pipeline. A background worker monitors memory pressure and proactively demotes least-recently-used context data.

### 3.3 Recompute vs. Fetch Decision

For some contexts, recomputing the KV cache from the raw prompt tokens may be faster than fetching compressed data from a lower tier:

```
Decision = if (recompute_latency < fetch_latency + decompress_latency):
               recompute()
           else:
               fetch_and_decompress()
```

| Context Size (Tokens) | Recompute Latency | T2 Fetch + Decompress | T3 Fetch + Decompress | Optimal Strategy |
|:---|:---|:---|:---|:---|
| 1K | ~10 ms | ~0.5 ms | ~15 ms | Always fetch from T2 |
| 10K | ~100 ms | ~2 ms | ~20 ms | Fetch from T2; recompute vs T3 depends on GPU load |
| 100K | ~1 s | ~15 ms | ~50 ms | Always fetch |
| 500K | ~5 s | ~60 ms | ~200 ms | Always fetch |

---

## 4. Cache Invalidation and Data Consistency

### 4.1 Invalidation Strategies

| Strategy | Mechanism | Consistency | Complexity | Recommended For |
|:---|:---|:---|:---|:---|
| **TTL-based** | Compressed blocks expire after fixed time | Eventual | Low | Session context with known expiration |
| **Event-driven** | Invalidate on session update/close events | Strong | Medium | Active session management |
| **Version-based** | Each update increments version; stale versions evicted | Strong | Medium | Multi-writer scenarios |
| **LRU eviction** | Evict least-recently-used when capacity exceeded | N/A (capacity management) | Low | All tiers |

### 4.2 Consistency Across Tiers

When the same context data exists at multiple tiers (e.g., a compressed copy on NVMe and an uncompressed copy in DRAM), consistency must be maintained:

- **Write-through:** Updates are written to all tiers synchronously. High consistency, high latency.
- **Write-back:** Updates are written to the highest tier and lazily propagated. Low latency, risk of data loss on crash.
- **Write-once (append-only):** Context data is immutable once written; new turns are appended as new compressed frames. No consistency issues. **Recommended** for conversation context, which is naturally append-only.

### 4.3 Dictionary Consistency

All tiers must have access to the same dictionary versions. A compressed frame on T3 (archived months ago) must still be decompressible with the dictionary version it was created with. **Solution:** Store dictionaries alongside the data in T3, or maintain a permanent dictionary registry that retains all historical versions.

---

## 5. Comparative Analysis of Tiered Compression Strategies

| Strategy | Description | Avg Ratio | Promotion Cost | Demotion Cost | Complexity | Best For |
|:---|:---|:---|:---|:---|:---|:---|
| **Uniform zstd L3** | Same compression at all tiers | 3.5× | None (no re-compression) | None | Low | Simple deployments |
| **Tiered levels** | L1 at T1, L3 at T2, L19 at T3 | 3.5–5.0× | Re-decompress/recompress | Re-compress at higher level | Medium | Cost-optimized production |
| **Hybrid zstd+OpenZL** | Zstd for T1/T2, OpenZL for T3 | 3.5–7.0× | Plan switch on tier transition | Plan application on demotion | High | Maximum ratio for archival |
| **Quantize+Compress** | INT8 quant on T0, zstd on T1+ | 4–10× (combined) | Dequantize + decompress | Quantize + compress | High | GPU-heavy workloads |

---

## 6. Performance Impact Analysis

### 6.1 End-to-End Latency Impact

For a typical inference request that requires fetching context from T1 (DRAM):

| Component | Without Compression | With Zstd L1 + Dict |
|:---|:---|:---|
| DRAM read (10 KB context) | ~0.5 μs | ~0.15 μs (3.3× smaller read) |
| Decompression | N/A | ~5 μs |
| Total read latency | ~0.5 μs | ~5.15 μs |
| Sessions per GB DRAM | ~40,000 | ~160,000 |
| Cost per session | ~$200/GB / 40K = $0.005 | ~$200/GB / 160K = $0.00125 |

**Trade-off:** Compression adds ~4.65 μs to read latency but enables 4× more sessions per GB of DRAM, reducing cost per session by 75%.

### 6.2 Throughput Impact

For a serving system handling 10,000 context fetches per second:

| Metric | Without Compression | With Compression |
|:---|:---|:---|
| DRAM bandwidth consumed | 100 MB/s (10K × 10 KB) | 30 MB/s (10K × 3 KB compressed) |
| CPU cores for decompression | 0 | ~0.3 cores (at 1.5 GB/s per core) |
| Net memory bandwidth saved | N/A | 70 MB/s |

Compression reduces memory bandwidth consumption by 70% at the cost of 0.3 CPU cores—a highly favorable trade-off in memory-bandwidth-constrained systems.

---

## 7. Identified Challenges and Solutions

| Challenge | Description | Solution |
|:---|:---|:---|
| **Re-compression overhead** | Moving data between tiers with different compression levels requires decompress→recompress | Avoid re-compression: store L3-compressed data at T1 (decompression speed is level-independent) |
| **Cold start promotion storms** | Many sessions resuming simultaneously causes burst reads from T2/T3 | Predictive pre-warming: use session access patterns to proactively promote likely-needed contexts |
| **Dictionary version sprawl** | Over time, many dictionary versions accumulate across tiers | Implement dictionary lifecycle policy: retain active + 2 previous versions; migrate old data to current dictionary during compaction |
| **Cross-tier monitoring** | Difficult to track compression ratios and performance across tiers | Unified metrics pipeline: export compression ratio, latency, and error rate per tier per data type |
| **GPU-CPU transfer bottleneck** | Decompression happens on CPU but data is needed on GPU | Use pinned (page-locked) memory for decompressed buffers; overlap decompression with GPU-CPU DMA transfer |

---

## 8. Emerging Trends and Future Directions

### 8.1 CXL Memory Pooling

Compute Express Link (CXL) technology creates a new "Tier 0.5" between GPU HBM and CPU DRAM: a shared, byte-addressable memory pool accessible by both CPUs and GPUs at DRAM-like latency. Compressed context data in CXL memory could be accessed by GPU-side decompression engines, bridging the current CPU-decompression bottleneck.

### 8.2 Transparent Compression at the Storage Layer

Next-generation NVMe controllers (Computational Storage Drives) include embedded compression engines that transparently compress/decompress data at the device level. This would make T2 compression "free" from the host CPU's perspective, converting the entire SSD tier into a compressed cache with no software overhead.

### 8.3 Memory-Semantic Compression

Research into "memory-semantic" compression treats compressed data as a first-class memory type with its own access semantics—enabling pointer traversal, field extraction, and predicate evaluation directly on compressed representations without full decompression.

---

## 9. Conclusion

Managing compressed LLM context across multi-tier memory systems requires a tier-aware compression strategy that matches compression algorithm, level, and format to each tier's capacity, bandwidth, and latency characteristics. The recommended approach is: (1) quantization for GPU HBM (T0); (2) fast zstd (L1–3) with dictionaries for CPU DRAM (T1); (3) moderate zstd (L3–7) with seekable format for NVMe (T2); and (4) maximum-ratio zstd (L10–19) or OpenZL for object storage (T3). The append-only, write-once semantics of conversation context naturally align with multi-tier caching by eliminating consistency complexity. Avoid re-compression on tier transitions by leveraging zstd's level-independent decompression speed. The net result is 3–4× effective capacity amplification at each tier, 70% memory bandwidth reduction, and 75% cost-per-session reduction, at the cost of ~5 μs additional latency per context fetch and approximately 0.3 CPU cores per 10K fetches/second.

---

## Glossary

| Term | Definition |
|:---|:---|
| **CXL** | Compute Express Link; a high-speed interconnect enabling shared, byte-addressable memory |
| **HBM** | High Bandwidth Memory; stacked DRAM used in GPUs and accelerators |
| **Capacity Amplification** | The effective increase in a tier's usable capacity achieved through compression |
| **Bandwidth Amplification** | The effective increase in data transfer throughput achieved by compressing data before transfer |
| **Tier Promotion** | Moving data from a lower (colder, higher-latency) tier to a higher (hotter, lower-latency) tier |
| **Tier Demotion** | Moving data from a higher tier to a lower tier, typically due to capacity pressure |
| **Write-Once Semantics** | A data model where entries are immutable after creation; updates are appended rather than modified in-place |
| **Computational Storage** | Storage devices with embedded processing units capable of performing computation (e.g., compression) on data in-place |

---

## References

[1] Collet, Y. & Kucherawy, M. (2021). "Zstandard Compression and the 'application/zstd' Media Type." RFC 8878. https://www.rfc-editor.org/rfc/rfc8878

[2] "LMCache: Multi-Tier KV Cache Management for LLM Inference." 2025. https://github.com/LMCache/LMCache

[3] "NVIDIA Dynamo: Distributed KV Cache Management." NVIDIA Developer Blog, 2025. https://developer.nvidia.com/

[4] "KV Cache Compression with Quantization." arXiv, 2024. https://arxiv.org/

[5] "Compute Express Link Consortium Specification." https://www.computeexpresslink.org/

[6] Samsung Electronics. "Computational Storage Drives." https://semiconductor.samsung.com/

[7] "Multi-Tier Memory Management for LLM Inference." NeurIPS 2025. https://neurips.cc/

[8] Facebook Engineering. "Zstd Seekable Format." https://github.com/facebook/zstd/tree/dev/contrib/seekable_format

[9] "io_uring: Efficient IO with Linux." https://kernel.dk/io_uring.pdf

[10] "NVIDIA H100 GPU Memory Architecture." https://www.nvidia.com/en-us/data-center/h100/
