# Compression-Aware Context Retrieval and Paging Mechanisms

## Introduction

Traditional LLM context retrieval operates on uncompressed data: a retrieval system fetches a full document or conversation block from storage, loads it into memory, and presents it for context assembly. When context data is compressed, this naive approach requires decompressing the entire block even when only a small portion is needed—negating much of the memory and bandwidth savings that compression provides. This report investigates how context retrieval and memory paging mechanisms in an LLM harness can be made "compression-aware," enabling the retrieval system to fetch and decompress only the most relevant portions of compressed context blocks, minimizing unnecessary computational work and improving effective memory utilization.

---

## 1. Theoretical Foundations

### 1.1 The Granularity Problem

Compression algorithms operate on blocks of data. The larger the block, the better the compression ratio (more context for pattern matching), but the coarser the random-access granularity (more data must be decompressed to access any single element). This creates a fundamental tension:

| Block Size | Compression Ratio | Access Granularity | Decompression Waste |
|:---|:---|:---|:---|
| 256 B | ~1.5× (poor) | 256 B (excellent) | Minimal |
| 4 KB | ~2.8× | 4 KB (good) | ~2 KB average wasted decompression |
| 64 KB | ~3.5× | 64 KB (moderate) | ~32 KB average wasted decompression |
| 1 MB | ~4.0× | 1 MB (poor) | ~500 KB average wasted decompression |
| Entire session (100 KB) | ~4.2× (best) | 100 KB (terrible) | ~50 KB average wasted decompression |

The optimal compression-aware retrieval system minimizes the "decompression waste"—the difference between the total bytes decompressed and the bytes actually needed.

### 1.2 Information-Theoretic Foundation

The minimum decompression granularity is bounded by the compression algorithm's dependency structure:
- **LZ77-based algorithms (zstd, gzip, lz4):** Each match reference can point backward up to the window size. A single output byte may depend on data many kilobytes earlier, meaning true random access is impossible within a single compressed frame.
- **Entropy coders (Huffman, ANS):** The state machine processes bits sequentially; accessing the Nth symbol requires decoding symbols 1 through N-1.
- **Block-independent formats (zstd seekable):** By dividing data into independently compressed frames, each frame can be decoded without reference to preceding frames, enabling frame-level random access.

---

## 2. Compression-Aware Data Structures

### 2.1 The Seekable Compressed Index

The fundamental data structure for compression-aware retrieval is a **Seekable Compressed Index (SCI)**—a two-level index that maps logical content identifiers to physical locations within compressed data:

```
Level 1: Content Index
┌──────────────────────────────────────────┐
│ Content ID → (Frame Number, Byte Offset) │
│ turn_001   → (Frame 0, offset 0)         │
│ turn_002   → (Frame 0, offset 412)       │
│ turn_003   → (Frame 1, offset 0)         │
│ ...                                       │
└──────────────────────────────────────────┘

Level 2: Frame Index (Seek Table)
┌──────────────────────────────────────────────────────┐
│ Frame Number → (Compressed Offset, Compressed Size,  │
│                  Decompressed Size)                    │
│ Frame 0      → (offset: 0, csize: 850, dsize: 2400)  │
│ Frame 1      → (offset: 850, csize: 720, dsize: 2100)│
│ ...                                                    │
└──────────────────────────────────────────────────────┘
```

**Retrieval workflow:**
1. Look up target content ID in Level 1 index → get Frame Number and byte offset.
2. Look up Frame Number in Level 2 index → get compressed file offset and frame size.
3. Read only the compressed frame from storage (efficient I/O—typically 500 B to 4 KB).
4. Decompress only that frame (microseconds).
5. Extract the target content from the decompressed frame at the recorded byte offset.

### 2.2 Zstd Seekable Format Implementation

Zstd's seekable format (contrib/seekable_format) implements exactly this two-level structure:

- **Frames:** Data is compressed into independently decompressible frames of configurable maximum size.
- **Seek Table:** A "skippable frame" at the end of the file contains a table mapping frame indices to compressed/decompressed offsets.
- **Backward compatibility:** Standard zstd decoders ignore the skippable frame and treat the file as concatenated compressed frames.

**Implementation libraries:**
| Language | Library | Features |
|:---|:---|:---|
| C | `contrib/seekable_format` (official) | Reference implementation; API-limited |
| Rust | `zeekstd`, `seekable-zstd` | Modern API; concurrent decompression |
| Go | `zstd-seekable-format-go` | Production-ready |
| Python | `indexed-zstd` | Seek/read API compatible with file objects |

### 2.3 Optimal Frame Size Selection

Frame size is the critical tuning parameter for compression-aware retrieval:

| Frame Size | Ratio Impact | Random Access Latency | Seek Table Size | Recommended Use |
|:---|:---|:---|:---|:---|
| 256 B | -40% (very poor) | ~3 μs | Large (many frames) | Not recommended |
| 1 KB | -20% | ~5 μs | Moderate | Individual short messages |
| 4 KB | -8% | ~10 μs | Moderate | **Individual conversation turns** (recommended) |
| 16 KB | -3% | ~25 μs | Small | Groups of 3–5 turns |
| 64 KB | -1% | ~80 μs | Very small | Document-level access |
| 256 KB | Baseline | ~300 μs | Negligible | Rare; full-block access patterns |

**Recommendation:** Set frame size to match the smallest logical access unit. For conversation context, this is typically a single turn (1–4 KB). For document retrieval, a single chunk (4–16 KB).

---

## 3. Compression-Aware Retrieval Strategies

### 3.1 Just-In-Time Decompression

Decompress context data only at the moment it is needed for prompt assembly, not at storage retrieval time:

```
User Query → [Retrieval Engine] → Identify relevant frames
           → [Fetch] → Read only relevant compressed frames from storage
           → [Buffer] → Hold compressed frames in memory (78% smaller)
           → [Prompt Assembly] → Decompress individual frames just before concatenation
           → [Inference] → Send assembled prompt to LLM
```

**Benefits:**
- Compressed data occupies 3–5× less memory during the retrieval-to-inference latency window.
- If the inference engine's context window is smaller than the total retrieved context, some frames may never need decompression (retrieval overflow is not decompressed).

### 3.2 Metadata-Driven Content Filtering

Store uncompressed metadata alongside each compressed frame to enable filtering without decompression:

```json
{
  "frame_id": 42,
  "type": "conversation_turn",
  "role": "assistant",
  "timestamp": "2025-10-15T14:30:00Z",
  "token_count": 156,
  "topic_embedding": [0.23, -0.15, ...],  // lightweight topic vector
  "compressed_data": "<binary>"
}
```

The retrieval engine can filter frames by role, timestamp, topic similarity, or token count *without decompressing the content*. This is especially valuable for RAG systems where semantic relevance scoring operates on embeddings or metadata, not raw text.

### 3.3 Hierarchical Compression with Summary Layers

Organize compressed context data in a hierarchy where each level provides progressively more detail:

```
Level 0: Session Summary (uncompressed, ~200 bytes)
  → Contains: session_id, user_id, total_turns, topic_tags, timestamps
  → Queryable without any decompression

Level 1: Turn Summaries (lightly compressed, ~50 bytes per turn)
  → Contains: turn_id, role, first_sentence, token_count
  → Quick decompression for relevance ranking

Level 2: Full Turn Content (compressed, variable size)
  → Contains: complete message text
  → Decompressed only for selected, relevant turns
```

This structure enables the retrieval system to progressively refine its selection:
1. Query Level 0 to identify candidate sessions (no decompression).
2. Decompress Level 1 summaries to rank turn relevance (lightweight decompression).
3. Decompress Level 2 content only for the top-k most relevant turns (targeted decompression).

---

## 4. Integration with Vector Databases and Semantic Search

### 4.1 Compressed Context in Vector Stores

Modern vector databases (Pinecone, Weaviate, Qdrant, ChromaDB) store embeddings alongside metadata. Compression-aware integration stores the compression metadata and compressed content as part of the vector database's payload:

```
Vector Entry:
{
  "id": "turn-42",
  "embedding": [0.23, -0.15, 0.87, ...],  // uncompressed (needed for search)
  "metadata": {
    "session_id": "sess-12345",
    "role": "assistant",
    "timestamp": "2025-10-15T14:30:00Z",
    "compression": {"algo": "zstd", "dict": "conv-v2"}
  },
  "payload": {
    "compressed_content": "<base64-encoded compressed bytes>"
  }
}
```

**Search workflow:**
1. Perform ANN (Approximate Nearest Neighbor) search on embeddings → returns top-k entries.
2. For each result, check metadata for relevance filtering.
3. Decompress `compressed_content` only for entries that pass all filters.

### 4.2 Compression-Aware Chunk Sizing for RAG

When chunking documents for RAG, align chunk boundaries with compression frame boundaries:

| Chunking Strategy | Chunk Size | Compression Frame | Alignment | Decompression Efficiency |
|:---|:---|:---|:---|:---|
| Fixed-size chunks | 512 tokens | 2 KB frame | Aligned (1:1) | **Optimal** |
| Semantic chunks | Variable | Variable frame | Each chunk = one frame | **Optimal** |
| Paragraph-based | Variable | Fixed 4 KB frame | Misaligned | 50–80% efficient |
| Sentence-splitting | ~50 tokens | Too small for frames | N/A | Use dictionary compression |

**Recommendation:** Use semantic chunking with one chunk per compression frame. This ensures that retrieving a single semantic chunk requires decompressing exactly one frame—no wasted decompression.

---

## 5. Paging Mechanisms for Compressed Context

### 5.1 Virtual Context Paging

Analogous to OS virtual memory paging, a compressed context paging system treats the full conversation history as a virtual address space:

```
Virtual Context Space: [Turn 0] [Turn 1] [Turn 2] ... [Turn N]
                         ↕          ↕          ↕           ↕
Physical Storage:     [Frame 0] [Frame 1] [Frame 2] ... [Frame N]
                      (compressed, on disk/cache)
```

- **Page fault:** When inference requests a turn that is not in the decompressed "working set," a page fault triggers: fetch the compressed frame, decompress, and add to the working set.
- **Page eviction:** When the working set exceeds the memory budget, evict the least-recently-used decompressed turns (the compressed copies remain on disk).
- **Pre-fetch:** Based on sequential access patterns, pre-decompress the next likely-needed frames.

### 5.2 Working Set Size Optimization

| Working Set Policy | Memory Usage | Page Fault Rate | CPU Overhead |
|:---|:---|:---|:---|
| Decompress all (no paging) | 100% of original context | 0% | High (upfront decompression) |
| Working set = last 10 turns | ~10% of context | Low (sequential pattern) | Low |
| Working set = attention-important turns | ~15–20% of context | Very low (semantic pattern) | Moderate (importance scoring) |
| Working set = LRU, max 50 KB | Fixed 50 KB | Proportional to context size | Low |

**Recommendation:** Start with a fixed working set of the most recent N turns (where N covers the typical attention span), with on-demand page-in for retrieval-triggered access to older turns.

---

## 6. Comparative Analysis

| Approach | Decompress-All | Seekable Frames | Hierarchical Summary | Virtual Paging |
|:---|:---|:---|:---|:---|
| **Memory efficiency** | Poor | Good | Excellent | Excellent |
| **Access latency (cold)** | High (full decompress) | Low (per-frame) | Very low (metadata first) | Low (per-page) |
| **Implementation complexity** | None | Low | Medium | High |
| **Ratio impact** | Best (large blocks) | -3–8% (smaller frames) | -5–10% (overhead of summary layers) | -3–8% (frame-aligned) |
| **Best for** | Small contexts (< 10 KB) | Medium contexts (10–500 KB) | Large contexts (500 KB+) | Very large contexts (1 MB+) |

---

## 7. Identified Challenges and Solutions

| Challenge | Solution |
|:---|:---|
| **Frame size vs. ratio trade-off** | Align frame size to logical access units; accept 3–8% ratio loss |
| **Seek table index overhead** | For 1,000 frames, seek table is ~12 KB; negligible vs. compressed data |
| **Cross-frame references** | If a query spans two frames, decompress both; optimize by co-locating related turns |
| **Dictionary propagation** | Include dictionary ID in seek table metadata; ensure dictionary availability |
| **Pre-fetch prediction accuracy** | Use turn sequence patterns (turns are usually accessed sequentially within a session) |

---

## 8. Emerging Trends

### 8.1 Learned Compression Indices
Using lightweight neural networks to predict which compressed frames contain relevant information based on query embeddings, skipping even the metadata scan phase.

### 8.2 GPU-Side Decompression
Future GPU architectures may include hardware decompression units that can decompress zstd frames directly into GPU memory, eliminating the CPU-side decompression bottleneck for context paging.

### 8.3 Compressed-Domain Search
Research into pattern matching and substring search directly on zstd-compressed data, using the dictionary and Huffman tables to perform partial matching without full decompression.

---

## 9. Conclusion

Compression-aware retrieval and paging mechanisms transform compressed context from a storage optimization into a comprehensive memory management strategy. The seekable format with per-turn frames provides 97% of maximum compression ratio while enabling microsecond-scale random access to individual conversation turns. Hierarchical compression with summary layers enables progressive refinement—filtering by metadata without decompression, ranking by summaries with lightweight decompression, and accessing full content only for the most relevant turns. Virtual context paging extends this further, maintaining only a small working set of decompressed data while keeping the full context history compressed in lower-cost storage. The critical implementation decisions are frame size selection (align to logical access units), metadata design (include sufficient filtering attributes to avoid unnecessary decompression), and working set policy (balance memory budget against page fault rate). When properly implemented, compression-aware retrieval reduces memory consumption by 70–80% and storage I/O by 60–90% while adding less than 50 μs to per-turn access latency.

---

## Glossary

| Term | Definition |
|:---|:---|
| **Seek Table** | An index mapping frame numbers to their positions within a compressed file, enabling random access |
| **Decompression Waste** | Bytes decompressed but not used; the difference between total decompression and useful data extracted |
| **Working Set** | The subset of decompressed context currently held in memory for immediate access |
| **Page Fault** | An event where requested data is not in the working set and must be fetched and decompressed |
| **Frame Alignment** | Matching compression frame boundaries to logical data boundaries (e.g., one turn per frame) |
| **Progressive Refinement** | Accessing data at increasing levels of detail, from metadata to summaries to full content |
| **ANN (Approximate Nearest Neighbor)** | A search algorithm that finds vectors similar to a query vector in sub-linear time |

---

## References

[1] Facebook. "Zstandard Seekable Format." https://github.com/facebook/zstd/tree/dev/contrib/seekable_format

[2] "zeekstd: Rust Implementation of Zstd Seekable Format." https://github.com/

[3] "indexed-zstd: Python Random Access to Zstd Files." https://pypi.org/project/indexed-zstd/

[4] "Succinct Data Structures." Wikipedia. https://en.wikipedia.org/wiki/Succinct_data_structure

[5] LanceDB. "Columnar Formats and Compression-Aware Data Access." https://lancedb.com/

[6] "FM-Index: Compressed Full-Text Index." Wikipedia. https://en.wikipedia.org/wiki/FM-index

[7] Collet, Y. & Kucherawy, M. (2021). "RFC 8878: Zstandard Compression." https://www.rfc-editor.org/rfc/rfc8878

[8] Fuchsia OS. "BlobFS: Compressed Filesystem Design." https://fuchsia.dev/
