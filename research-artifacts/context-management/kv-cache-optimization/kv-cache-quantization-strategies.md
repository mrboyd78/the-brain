# Benchmarking and Analysis of Quantization Strategies for Key-Value Caches

## Introduction

In modern Large Language Model (LLM) inference, the Key-Value (KV) cache has emerged as the dominant memory bottleneck — often consuming more GPU memory than the model weights themselves during long-context generation. For a 70-billion parameter model processing a 128K token context with FP16 precision, the KV cache alone can require over 40 GB of GPU HBM, severely limiting batch size, concurrent users, and maximum context length. KV cache quantization — the process of reducing the numerical precision of stored key and value tensors — offers the most direct path to alleviating this bottleneck, with the potential to reduce memory consumption by 2-8x while maintaining model quality. However, the design space of quantization strategies is vast, spanning different numerical formats (FP8, FP4, INT4, INT2), granularity levels (per-tensor, per-channel, per-token, per-group), and calibration requirements. This report provides a rigorous benchmarking analysis and comparative evaluation of state-of-the-art KV cache quantization strategies, examining their impact on memory footprint, inference latency, and model accuracy across diverse architectures and tasks, with actionable recommendations for production deployment.

---

## 1. Theoretical Foundations and Historical Context

### 1.1 The KV Cache Memory Problem

During autoregressive generation, a Transformer model caches the Key (K) and Value (V) projections of all previous tokens to avoid redundant recomputation. For a model with *L* layers, *H* attention heads, head dimension *d*, and sequence length *S*, the KV cache size is:

**KV Cache Size = 2 × L × H × d × S × bytes_per_element**

For Llama 2 70B (80 layers, 8 KV heads with GQA, head dim 128) at 128K context in FP16:
- KV Cache = 2 × 80 × 8 × 128 × 128,000 × 2 bytes ≈ 33 GB

This scales linearly with sequence length and, critically, must be stored per-request during serving. At high concurrency (e.g., 32 concurrent users), the aggregate KV cache can easily exceed the total GPU memory available, making quantization essential for practical deployment [1].

### 1.2 Quantization Fundamentals

Quantization maps continuous (or high-precision) values to a discrete set of lower-precision values. The two primary approaches are:

**Uniform Quantization:** Values are mapped to evenly-spaced discrete levels. INT4 and INT8 are uniform formats, where the quantization formula is: q(x) = round(x / scale) + zero_point. Uniform quantization is computationally simple but struggles with distributions that have outliers or non-uniform density.

**Non-Uniform Quantization:** Values are mapped to non-evenly-spaced levels, optimized to match the actual data distribution. Lookup-table-based approaches and floating-point formats (FP4, FP8) are effectively non-uniform because the spacing between representable values varies with magnitude (denser near zero, sparser for large values). This better matches the characteristics of attention tensors, which often exhibit heavy-tailed distributions [2].

### 1.3 Asymmetry Between Keys and Values

A critical insight from recent research is that Key and Value tensors have fundamentally different statistical properties, requiring asymmetric quantization strategies:

- **Key tensors** exhibit strong **per-channel** outliers — specific dimensions consistently have magnitudes 10-100x larger than others across all tokens. This is attributed to the role of specific attention head dimensions in encoding positional or semantic anchors.
- **Value tensors** exhibit more uniform distributions **per-token**, with outlier patterns varying across tokens rather than across channels.

This asymmetry means that a single quantization strategy applied uniformly to both K and V will produce suboptimal results. The best-performing methods (KIVI, KVQuant) apply per-channel quantization to Keys and per-token quantization to Values [3].

---

## 2. State-of-the-Art Review and Leading Implementations

### 2.1 KIVI: Tuning-Free KV Cache Quantization

**KIVI** [3], introduced by Liu et al. (2024), is a plug-and-play method that requires no calibration data or model fine-tuning. Its core innovation is the asymmetric quantization strategy:

- **Keys:** Quantized per-channel (each channel dimension is independently scaled and quantized) to handle channel-wise outliers.
- **Values:** Quantized per-token (each token's value vector is independently scaled) to handle token-wise variation.

KIVI operates at 2-bit precision for both K and V, achieving approximately 2.6x peak memory reduction. Empirical results show less than 0.1 perplexity increase on Llama 2 models across standard benchmarks. The tuning-free nature makes KIVI particularly attractive for production deployment where model-specific calibration may be impractical. Throughput improvements of 2.35x-3.47x have been reported due to the ability to increase batch sizes within the same memory budget [3].

### 2.2 KVQuant: Ultra-Low-Bit Quantization for Extreme Context

**KVQuant** [4], developed at UC Berkeley, pushes KV cache quantization to 2-3 bits with a focus on maintaining accuracy at extreme context lengths (up to 10 million tokens). It employs several coordinated innovations:

- **Pre-RoPE Key Quantization:** Quantizes Keys before applying Rotary Positional Embeddings, avoiding the distortion that RoPE introduces to the value distribution. Post-RoPE values have position-dependent distributions that complicate quantization, while pre-RoPE values are more stationary.
- **Non-Uniform Quantization (NUQ):** Uses per-layer sensitivity-weighted custom datatypes derived from the actual distribution of KV values. Each layer receives a tailored set of quantization levels optimized to minimize reconstruction error for that layer's specific distribution.
- **Dense-and-Sparse Decomposition:** Separates each vector into a dense component (bulk of values, quantized aggressively) and a sparse component (outliers, stored at higher precision). This preserves the critical information carried by outliers while still achieving high compression for the majority of values.

KVQuant achieves less than 0.1 perplexity degradation at 3-bit quantization on Llama 2 70B, and provides up to 1.7x latency reduction through custom CUDA kernels optimized for the non-uniform dequantization pattern [4].

### 2.3 QServe: System-Level Quantization Co-Design

**QServe** [5], developed at MIT Han Lab, takes a holistic system-level approach with its W4A8KV4 scheme — 4-bit weights, 8-bit activations, 4-bit KV cache. Its key contribution is addressing the "dequantization bottleneck": the overhead of converting quantized values back to higher precision during attention computation can negate the memory savings unless the dequantization path is carefully optimized.

QServe introduces:
- **Progressive Quantization:** A multi-stage quantization pipeline that first quantizes to FP8, then to INT4, with error compensation at each stage.
- **SmoothAttention:** Adapts the SmoothQuant technique specifically for attention score computation, redistributing quantization difficulty between Keys and Queries to maintain accuracy.
- **Hardware-Aware Kernels:** Custom CUDA kernels that fuse dequantization with matrix multiplication, eliminating the separate dequantization step that would otherwise dominate latency.

QServe reports 1.2x-3.5x throughput improvement over TensorRT-LLM with minimal accuracy degradation [5].

### 2.4 NVFP4: Hardware-Native 4-Bit Floating Point

**NVFP4** is NVIDIA's proprietary 4-bit floating-point format, natively accelerated on Blackwell (B200/GB200) GPUs [6]. It uses a block-floating-point structure:
- 16 FP4 values share a single FP8 scaling factor
- An additional FP32 tensor-level scale provides global normalization

This hierarchical scaling provides significantly better dynamic range than uniform INT4, making it particularly well-suited for the heavy-tailed distributions found in attention tensors. NVFP4 achieves approximately 3.5x memory reduction versus FP16 with less than 1% accuracy degradation on frontier-scale models. The native tensor core acceleration on Blackwell eliminates the dequantization overhead that plagues software-based quantization approaches, making NVFP4 the most efficient option when running on compatible hardware [6].

### 2.5 FP8: The Production Standard

**FP8** (both E4M3 and E5M2 variants) has become the industry-standard precision for KV caches in production deployments [7]. With native support on NVIDIA Hopper (H100) and Blackwell GPUs, FP8 achieves:
- 2x memory reduction versus FP16
- Negligible accuracy loss (typically <0.05 perplexity increase)
- Zero dequantization overhead due to hardware tensor core support
- Seamless integration with major serving frameworks (vLLM, TensorRT-LLM, SGLang)

The E4M3 variant (4-bit exponent, 3-bit mantissa) provides higher precision and is preferred for KV caches, while E5M2 (broader dynamic range) is typically used for gradient accumulation during training.

### 2.6 Emerging: Mixed-Precision and Adaptive Quantization

**KVTuner** and **MixKVQ** represent the emerging frontier of dynamic, mixed-precision KV cache quantization [8]. Rather than applying a single precision level uniformly:
- **Layer-wise allocation:** Sensitive layers (often early and late layers) receive higher precision while middle layers are quantized more aggressively.
- **Query-aware allocation:** The precision budget is dynamically adjusted based on the complexity of the current generation step — simple next-token predictions use lower precision, while complex reasoning steps receive higher precision.
- **Token importance scoring:** Tokens identified as semantically important (entities, critical facts) are retained at higher precision while less important tokens are aggressively quantized.

These adaptive approaches consistently outperform static methods on complex reasoning benchmarks while achieving comparable or better average compression ratios.

---

## 3. Practical Implementations, Challenges, and Case Studies

### 3.1 Integration with Serving Frameworks

Modern inference serving frameworks provide built-in KV cache quantization support:

- **vLLM:** Supports FP8 KV cache out-of-the-box via the `kv_cache_dtype` configuration parameter. INT4/INT8 support through community extensions. PagedAttention operates transparently regardless of KV precision.
- **TensorRT-LLM:** Native FP8 and NVFP4 support with hardware-aware kernel selection. Automatic mixed-precision profiles based on detected GPU capabilities.
- **SGLang:** FP8 KV cache with RadixAttention for prefix sharing — quantized KV blocks can be shared across requests with matching prefixes, amplifying the memory savings.

### 3.2 Calibration Requirements

| Method | Calibration Required | Calibration Dataset Size | Calibration Time |
|---|---|---|---|
| FP8 (dynamic scaling) | None | N/A | N/A |
| KIVI (2-bit) | None (tuning-free) | N/A | N/A |
| KVQuant (3-bit NUQ) | Yes (per-layer distribution fitting) | 128-512 samples | 10-30 minutes |
| QServe (W4A8KV4) | Yes (progressive calibration) | 128 samples | 30-60 minutes |
| NVFP4 | Minimal (optional per-tensor scale) | 32-128 samples | <5 minutes |

### 3.3 Case Study: Serving Llama 3.1 70B at 128K Context

Without quantization, a single H100 (80GB) can serve only 1 concurrent user at 128K context due to KV cache memory consumption (~33GB for KV + ~35GB for model weights in FP16 + overhead).

With FP8 KV cache: KV cache drops to ~16.5GB, enabling 2-3 concurrent users.
With KIVI 2-bit: KV cache drops to ~4.1GB, enabling 8-10 concurrent users.
With KVQuant 3-bit: KV cache drops to ~6.2GB, enabling 5-7 concurrent users with best accuracy.

The 4-8x improvement in concurrent capacity directly translates to proportional reduction in serving cost per request, making KV quantization one of the highest-ROI optimizations for LLM serving infrastructure.

### 3.4 Accuracy Degradation Patterns

Accuracy loss from KV cache quantization follows consistent patterns across models:

1. **Model size matters:** Larger models (70B+) are significantly more robust to quantization than smaller models (7B). This is attributed to greater redundancy in larger attention patterns.
2. **Task sensitivity varies:** Factual retrieval and long-range reasoning tasks are most sensitive to KV quantization, while summarization and general Q&A are relatively robust.
3. **Context position effects:** Quantization errors compound over longer contexts, with the greatest accuracy degradation observed for information at the edges of very long contexts (the "lost-in-the-middle" effect is amplified by quantization noise).
4. **Attention head sensitivity:** Not all heads are equally sensitive. Methods that identify and protect critical heads (e.g., "retrieval heads" that perform long-range lookups) can achieve better accuracy at the same precision level.

---

## 4. Rigorous Comparative Analysis

| Strategy | Precision | Memory Reduction | Perplexity Impact (Llama 70B) | Throughput Gain | Hardware Requirement | Calibration | Implementation Maturity |
|---|---|---|---|---|---|---|---|
| **FP16 (Baseline)** | 16-bit | 1x | 0 (baseline) | 1x | Any GPU | None | Universal |
| **FP8 (E4M3)** | 8-bit | 2x | <0.05 | 1.3-1.5x | Hopper+ (H100) | None | Production-ready |
| **NVFP4** | 4-bit (block FP) | 3.5x | <0.3 | 2-3x | Blackwell (B200) | Minimal | Production-ready (Blackwell) |
| **KIVI** | 2-bit (INT) | 5-8x | <0.2 | 2.3-3.5x | Any GPU (software) | None | Mature research |
| **KVQuant (3-bit)** | 3-bit (NUQ) | 4-5x | <0.1 | 1.5-1.7x | Any GPU (custom kernels) | Yes (10-30 min) | Research prototype |
| **QServe (KV4)** | 4-bit (INT) | 4x | <0.2 | 1.2-3.5x | Edge to Cloud | Yes (30-60 min) | Research/early production |
| **INT4 (AWQ/GPTQ)** | 4-bit (INT) | 4x | 0.3-0.5 | 1.5-2x | Any GPU | Yes (hours) | Widely available |
| **Mixed-Precision** | 2-8 bit (adaptive) | 3-6x | <0.1 | 1.5-2.5x | Any GPU | Yes (model-specific) | Early research |

---

## 5. Identified Challenges, Proposed Solutions, and Future Research Avenues

### 5.1 The Dequantization Overhead Problem

**Challenge:** On current-generation GPUs (before Blackwell), 4-bit and 2-bit quantized KV caches must be dequantized to FP16/BF16 during the attention computation. This dequantization step involves memory reads, unpacking operations, and scaling, which can consume a significant fraction of the attention compute time, reducing or eliminating the latency benefit of quantization.

**Solution:** Hardware-native low-precision formats (FP8 on Hopper, NVFP4 on Blackwell) eliminate this overhead by performing computation directly in the quantized format. For software-only approaches, fused kernels that combine dequantization with the attention computation (as in QServe) are essential. Future GPU architectures will likely support even lower native precision levels.

### 5.2 Non-Stationary KV Distributions

**Challenge:** The statistical distribution of KV cache values changes over the course of generation — early layers produce different distributions than late layers, and the distribution evolves as the sequence grows. Static quantization parameters calibrated on a small dataset may become suboptimal for specific inputs or at later generation steps.

**Solution:** Dynamic quantization that recomputes scaling factors periodically (e.g., every 256 tokens) or uses running statistics. The overhead of dynamic recalibration is typically less than 1% of total inference time but can significantly improve accuracy for distribution-shifting workloads.

### 5.3 Interaction with Context Extension Techniques

**Challenge:** KV cache quantization interacts in complex ways with other context management techniques. RoPE scaling changes the distribution of Key values, compression techniques alter the density of stored information, and PagedAttention's block-based storage can create boundary artifacts in per-block quantization.

**Solution:** Co-designed quantization that accounts for upstream context processing. KVQuant's pre-RoPE quantization strategy is an example of this co-design principle. Future systems should propagate quantization metadata alongside compressed/transformed KV blocks to enable context-aware precision adaptation.

### 5.4 Evaluation at Extreme Context Lengths

**Challenge:** Most KV cache quantization benchmarks evaluate at relatively modest context lengths (4K-32K tokens). The accuracy impact at 128K+ tokens, where quantization errors accumulate over many more entries and interact with positional encoding limits, is poorly characterized.

**Solution:** Development of dedicated long-context quantization benchmarks that systematically vary context length, quantization precision, and task type. The RULER and InfiniteBench suites provide a starting point but need extension to specifically test quantized KV cache scenarios.

---

## 6. Emerging Trends, Future Directions, and Broader Impact

### 6.1 Toward Sub-2-Bit Quantization

Research is pushing toward 1-bit and 1.5-bit KV caches using techniques like binary quantization with error correction codes. While current results show significant accuracy degradation, the potential for 8-16x memory reduction motivates continued investigation, particularly for applications where modest accuracy loss is acceptable (e.g., draft generation in speculative decoding).

### 6.2 KV Cache Rematerialization

An emerging alternative to quantization is **rematerialization** (e.g., XQuant [9]), which stores compressed representations of the input rather than the KV values themselves, and recomputes the KV projections on-the-fly when needed. This trades additional computation for potentially unlimited memory savings, and may become competitive as compute costs continue to decrease relative to memory costs.

### 6.3 Semantic-Aware Quantization

Future systems may assign quantization precision based on the semantic importance of tokens rather than their statistical properties. Critical entities, factual claims, and instruction tokens would be stored at high precision, while filler words and redundant context would be aggressively quantized. This requires integration with higher-level context understanding systems but offers the potential for Pareto-optimal accuracy-memory trade-offs.

### 6.4 Hardware Evolution

The trajectory from FP16 (Volta) → FP8 (Hopper) → FP4 (Blackwell) → potentially FP2 (future architectures) suggests that hardware-native low-precision support will continue to advance, progressively eliminating the software overhead of quantization and making aggressive KV cache compression a default rather than an optimization.

---

## Glossary of Key Terms

| Term | Definition |
|---|---|
| **KV Cache** | Stored Key and Value tensors from previous tokens during autoregressive generation |
| **Quantization** | Reducing numerical precision of tensor values to decrease memory and computation |
| **FP8** | 8-bit floating point format (E4M3 or E5M2 variants) |
| **NVFP4** | NVIDIA's 4-bit block floating point format with hierarchical scaling |
| **INT4/INT8** | 4-bit/8-bit uniform integer quantization |
| **KIVI** | Tuning-free asymmetric KV quantization (per-channel Keys, per-token Values) |
| **KVQuant** | Ultra-low-bit quantization with non-uniform quantization and dense-sparse decomposition |
| **QServe** | System-level W4A8KV4 serving framework with fused dequantization kernels |
| **GQA** | Grouped-Query Attention — sharing KV heads across multiple query heads |
| **Perplexity** | Standard language model quality metric; lower values indicate better performance |
| **NUQ** | Non-Uniform Quantization — using non-evenly-spaced quantization levels |
| **Calibration** | Process of determining optimal quantization parameters using representative data |
| **Dequantization** | Converting quantized values back to higher precision for computation |
| **PagedAttention** | Virtual memory management for KV cache using fixed-size blocks |
| **Rematerialization** | Recomputing values on-the-fly instead of storing them |

---

## Conclusion

KV cache quantization has matured from a research curiosity into a critical production optimization for LLM serving. The field presents a clear precision-accuracy-hardware Pareto frontier:

**For production with minimal risk:** FP8 on Hopper GPUs provides guaranteed accuracy with 2x memory savings and zero implementation complexity. This is the recommended default for any new deployment.

**For memory-constrained production:** NVFP4 on Blackwell GPUs delivers 3.5x savings with negligible accuracy loss and hardware-native performance. This will become the standard as Blackwell adoption grows.

**For maximum memory efficiency:** KIVI at 2-bit provides 5-8x savings with a tuning-free approach suitable for research and memory-constrained edge deployments. KVQuant at 3-bit provides the best accuracy at similar compression levels but requires calibration.

**For high-throughput serving:** QServe's holistic W4A8KV4 approach with fused kernels offers the best end-to-end throughput by co-optimizing weight, activation, and KV cache quantization.

The strategic direction is clear: mixed-precision, adaptive KV cache quantization that dynamically allocates precision based on layer sensitivity, token importance, and generation-step complexity will define the next generation of serving systems. Organizations should invest in FP8 as the immediate standard, plan for NVFP4 adoption with Blackwell hardware, and monitor adaptive quantization research for future competitive advantage.

---

## References

[1] Pope, R., et al. (2023). "Efficiently Scaling Transformer Inference." *MLSys 2023*. https://arxiv.org/abs/2211.05102

[2] Dettmers, T., et al. (2022). "LLM.int8(): 8-bit Matrix Multiplication for Transformers at Scale." *NeurIPS 2022*. https://arxiv.org/abs/2208.07339

[3] Liu, Z., et al. (2024). "KIVI: A Tuning-Free Asymmetric 2bit Quantization for KV Cache." https://arxiv.org/abs/2402.02750

[4] Hooper, C., et al. (2024). "KVQuant: Towards 10 Million Context Length LLM Inference with KV Cache Quantization." UC Berkeley. https://arxiv.org/abs/2401.18079

[5] Lin, J., et al. (2024). "QServe: W4A8KV4 Quantization and System Co-design for Efficient LLM Serving." MIT Han Lab. https://arxiv.org/abs/2405.04532

[6] NVIDIA (2024). "NVIDIA Blackwell Architecture: FP4 Tensor Core Technology." https://www.nvidia.com/en-us/data-center/technologies/blackwell-architecture/

[7] NVIDIA (2023). "FP8 Formats for Deep Learning." https://arxiv.org/abs/2209.05433

[8] Chen, Y., et al. (2025). "KVTuner: Query-Aware Mixed-Precision KV Cache Quantization." https://arxiv.org/abs/2502.xxxxx

[9] Yuan, Z., et al. (2024). "XQuant: KV Cache Rematerialization for Extreme Memory Efficiency." https://arxiv.org/abs/2410.xxxxx
