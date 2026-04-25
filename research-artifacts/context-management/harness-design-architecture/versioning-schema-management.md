# Versioning and Schema Management for Context Harnesses

## Introduction

Context harnesses evolve continuously — prompt templates change, context schemas are extended, compression algorithms are upgraded, retrieval configurations are tuned, and new context sources are added. Without disciplined versioning and schema management, these changes create a cascade of failures: existing conversations break when prompt formats change, stored context becomes unreadable when schemas evolve, and A/B tests produce invalid comparisons when pipeline versions differ silently. This report provides a comprehensive analysis of versioning and schema management for context harnesses, covering prompt versioning, context schema evolution, backward compatibility, migration strategies, and the operational practices that enable teams to evolve context systems rapidly without breaking production.

---

## 1. What Needs Versioning

### 1.1 Versionable Components

Every mutable component of the context harness requires version management:

| Component | What Changes | Change Frequency | Impact of Unversioned Change |
|---|---|---|---|
| **System Prompt** | Instructions, persona, constraints | Weekly | Quality regression, behavior change |
| **Context Schema** | Field names, types, relationships | Monthly | Deserialization failures, data corruption |
| **Retrieval Config** | Index, chunk size, top-K, reranker | Weekly | Quality/latency change |
| **Compression Config** | Algorithm, ratio, parameters | Monthly | Information loss, quality change |
| **Budget Allocation** | Token limits per component | Weekly | Context overflow or underutilization |
| **Model Config** | Model version, temperature, params | Monthly | Quality/cost change |
| **Pipeline Config** | Component ordering, feature flags | Weekly | Processing errors, quality change |

### 1.2 Version Identifier Design

Effective version identifiers for context components:

- **Semantic Versioning (MAJOR.MINOR.PATCH):** `v1.2.3` — Major: breaking changes, Minor: new features, Patch: bug fixes.
- **Content Hashing:** SHA-256 of the component content. Automatically detects any change, no manual version bumping required.
- **Timestamp-Based:** ISO 8601 timestamp of last modification. Simple but doesn't convey change magnitude.
- **Hybrid:** Semantic version + content hash. Provides both human-readable versioning and automatic change detection.

**Recommendation:** Use semantic versioning for externally-visible components (system prompts, API schemas) and content hashing for internal components (retrieval configs, compression parameters).

---

## 2. Schema Management

### 2.1 Context Schema Definition

A context schema defines the structure of context blocks flowing through the harness:

```
ContextBlock v2.1:
  source: string (required)
  content: string (required)
  tokenCount: integer (required)
  priority: float (required, 0.0-1.0)
  metadata:
    retrievalScore: float (optional)
    timestamp: datetime (optional)
    sourceType: enum[retrieval, history, memory, system, tool] (required)
    compressionRatio: float (optional, added in v2.1)
  provenance:
    provider: string (required)
    version: string (required, added in v2.0)
```

### 2.2 Schema Evolution Strategies

**Additive Evolution (Non-Breaking):**
- Add optional fields with default values
- Add new enum values to existing enums
- Increase length limits
- Example: Adding `compressionRatio` as optional field with default `null`

**Transformative Evolution (Potentially Breaking):**
- Rename fields (breaking: old code uses old name)
- Change field types (breaking: old code expects old type)
- Make optional fields required (breaking: old records lack the field)
- Remove fields (breaking: old code references removed field)

**Migration Strategies for Breaking Changes:**

| Strategy | Description | Downtime | Complexity |
|---|---|---|---|
| **Big Bang** | Deploy new schema, migrate all data | Yes | Low |
| **Dual Write** | Write to both old and new schema simultaneously | No | Moderate |
| **Expand-Contract** | Add new fields → migrate → remove old fields | No | Moderate |
| **Schema Registry** | Versioned schemas with on-read transformation | No | High |

### 2.3 Schema Registry Pattern

A centralized schema registry manages all context schemas:

- **Registration:** Every schema version is registered with metadata (version, author, changelog, compatibility).
- **Compatibility Checking:** Before registering a new schema version, the registry validates backward compatibility (readers using old schema can read new data).
- **On-Read Transformation:** When reading data written with an older schema version, apply transformation functions that upgrade the record to the current schema.
- **Schema Lineage:** Track the evolution chain of each schema, enabling rollback and historical analysis.

---

## 3. Prompt Versioning

### 3.1 Prompt-as-Code

System prompts and prompt templates should be managed with the same rigor as source code:

- **Version Control:** Store prompts in git alongside application code.
- **Review Process:** Prompt changes go through code review with testing requirements.
- **Deployment Pipeline:** Prompts deploy through staging → canary → production, not by editing production directly.
- **Rollback Capability:** Any prompt version can be rolled back instantly.

### 3.2 Prompt Version Lifecycle

```
Draft → Review → Test (eval suite) → Canary (5% traffic) → Gradual Rollout → Full Production → Archived
```

**Testing Requirements:**
- Run prompt against evaluation suite (accuracy, faithfulness, safety).
- Compare metrics against previous version (A/B test with statistical significance).
- Check for regressions on known failure cases.
- Validate token count fits budget at maximum context length.

### 3.3 Prompt Parameterization

Separate prompt structure from content:

- **Template:** The structural prompt with placeholders (`{system_instructions}`, `{user_context}`, `{retrieved_docs}`)
- **Parameters:** The variable content that fills placeholders (system instructions text, context blocks)
- **Configuration:** Template-parameter binding (which system instructions version to use with which template version)

This separation enables:
- Changing system instructions without changing template structure
- Testing template changes independently from content changes
- A/B testing parameter variants within the same template

---

## 4. Pipeline Versioning

### 4.1 Pipeline Configuration as Data

The entire context pipeline configuration — which components to use, in what order, with what parameters — should be versionable:

```yaml
pipeline_version: "3.2.0"
components:
  retrieval:
    provider: "hybrid_search_v2"
    config:
      dense_model: "bge-large-en-v1.5"
      sparse_weight: 0.3
      top_k: 10
  reranking:
    provider: "cohere_rerank_v3"
    config:
      top_n: 5
  compression:
    provider: "llmlingua2"
    config:
      ratio: 0.5
  budget:
    total_tokens: 16384
    allocation:
      system: 1000
      retrieval: 6000
      history: 4000
      response: 5384
```

### 4.2 Immutable Pipeline Snapshots

Each request should record the exact pipeline configuration version used, enabling:
- **Reproducibility:** Replay any request with the exact same configuration.
- **Attribution:** When quality changes, identify which configuration change caused it.
- **Comparison:** Compare metric distributions across pipeline versions.
- **Rollback:** Revert to a specific pipeline version if quality regresses.

---

## 5. Backward Compatibility and Migration

### 5.1 Compatibility Matrix

| Change Type | Backward Compatible? | Migration Required? | Example |
|---|---|---|---|
| **New optional field** | Yes | No | Adding `compressionRatio` with default |
| **Default value change** | Yes | No | Changing default `priority` from 0.5 to 0.7 |
| **Field rename** | No | Yes | `source` → `provider` |
| **Type change** | No | Yes | `priority: int` → `priority: float` |
| **Field removal** | No | Yes | Removing deprecated `legacyScore` |
| **New required field** | No | Yes | Adding required `version` to all blocks |

### 5.2 Migration Best Practices

- **Never skip versions:** Migrate v1 → v2 → v3, not v1 → v3 directly. Each migration step is simpler and testable.
- **Idempotent migrations:** Running a migration twice should produce the same result as running it once.
- **Reversible migrations:** Every migration should have a corresponding rollback script.
- **Progressive migration:** Migrate in batches with validation between batches, not all-at-once.
- **Shadow verification:** During migration, run both old and new schemas in parallel and compare results.

---

## 6. Challenges and Future Directions

### 6.1 Cross-System Schema Coordination

**Challenge:** Context schemas span multiple systems (application, retrieval, memory, inference). Schema changes must be coordinated across all systems simultaneously.

**Solution:** Schema registry as the single source of truth. All systems read schemas from the registry at startup and validate compatibility before accepting changes.

### 6.2 LLM-Specific Versioning Challenges

**Challenge:** When the underlying LLM is updated, the optimal prompt format may change — a prompt optimized for GPT-4 may not work optimally for GPT-4o. Model updates effectively create implicit context schema changes.

**Solution:** Model-prompt compatibility matrices that track which prompt versions are validated for which model versions. Automated re-evaluation when models are updated.

---

## Glossary of Key Terms

| Term | Definition |
|---|---|
| **Semantic Versioning** | Version numbering conveying change magnitude (MAJOR.MINOR.PATCH) |
| **Schema Registry** | Centralized service managing versioned data schemas |
| **Expand-Contract** | Migration pattern: add new → migrate data → remove old |
| **Prompt-as-Code** | Managing prompts with software engineering practices |
| **Content Hashing** | Version identification via cryptographic hash of content |
| **Backward Compatibility** | New versions can read data written by old versions |
| **Pipeline Snapshot** | Immutable record of exact pipeline configuration per request |

---

## Conclusion

Versioning and schema management are the foundation of reliable context harness evolution. Without them, every change is a potential production outage, every A/B test is potentially comparing different (unknown) configurations, and every debugging session lacks the reproducibility needed for effective diagnosis.

For production implementation: adopt prompt-as-code practices immediately (version control, review, testing), implement pipeline configuration versioning (immutable snapshots per request), and establish a schema evolution strategy (expand-contract for backward compatibility, schema registry for coordination). These practices transform context harness development from a fragile, error-prone process into a disciplined engineering practice.

---

## References

[1] Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly Media.

[2] Apache Avro. "Schema Evolution and Compatibility." https://avro.apache.org

[3] Confluent. "Schema Registry Documentation." https://docs.confluent.io/platform/current/schema-registry/

[4] Various (2024-2025). "Prompt Engineering Lifecycle and Version Management Best Practices."
