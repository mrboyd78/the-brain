# Secure Access Control and Authorization for Context Harnesses

## Introduction

As Context Harnesses evolve to support multi-tenant applications and enterprise AI agents, security transitions from a perimeter-defense model to a zero-trust embedded model. LLMs process text agnostically; they do not inherently understand permissions. If a context harness retrieves sensitive HR documents alongside public company policies because it lacks strict access control, the LLM will happily synthesize and reveal that private data to an unauthorized user ("Data Exfiltration via Prompt"). This report explores sophisticated Access Control and Authorization strategies for context management pipelines, focusing on Role-Based Access Control (RBAC) integration in vector stores, identity propagation, contextual boundary enforcement, and token-level authorization mechanisms.

---

## 1. The Authorization Challenge in Generative AI

Traditional web applications enforce authorization at the database query or API endpoint layer. In RAG and Agentic systems, the challenge is fundamentally different:
- **Indirect Access:** The user does not query the database directly. The harness queries the database based on the user's prompt semantic intent.
- **The Confused Deputy:** The LLM acts as an intermediary. A malicious user prompt can trick the LLM (the deputy) into retrieving and revealing data it has access to, but the user does not.
- **Implicit Leakage:** Even if direct facts are redacted, the LLM might infer and leak sensitive details by connecting seemingly unrelated unclassified documents provided in the context window.

---

## 2. Identity Propagation and Zero-Trust Gathering

The context harness must operate under a strict zero-trust model, ensuring that every downstream service (Vector DB, Graph DB, API integration) knows precisely *who* is requesting the data.

### 2.1 Full-Stack Identity Propagation

When a request enters the harness, the user's authentication token (e.g., a JWT) must be propagated to every context gathering mechanism.
- Instead of the harness using a global "Service Account" to query the Vector DB, it should use the user's propagated identity.
- **Implementation:** The harness extracts the User ID, Tenant ID, and Group roles from the incoming JWT and injects them as mandatory filters in every sub-query.

### 2.2 Metadata-Driven RBAC in RAG

Vector databases (like Pinecone, Qdrant, Milvus) do not support traditional SQL `GRANT` tables. Authorization must be engineered via metadata filtering.

**The Strategy:**
1. **At Ingestion:** When appending a document to the vector store, tag its embedding with rigid `tenant_id` and `allowed_roles` metadata arrays.
2. **At Retrieval:** The harness intercepts the semantic query and forces a pre-filter:
   `db.query(vector=user_query, filter={"tenant_id": current_tenant, "roles": {"$in": user_roles}})`
3. **Execution:** The vector search only executes across the secure subset of documents, mathematically guaranteeing that unauthorized documents are never retrieved, regardless of their semantic similarity to the prompt.

---

## 3. Contextual Boundary Enforcement

Merely retrieving the right documents is insufficient if the prompt design allows the user to break out of their operational constraints.

### 3.1 Hardened System Prompts

The system prompt is the primary declarative authorization boundary for the LLM.
- **Dynamic Policy Injection:** Do not use static system prompts. The harness should dynamically generate the system prompt based on the user's authorization tier.
  - *Standard User:* "You can answer questions based on the retrieved docs."
  - *Admin User:* "You can answer questions and execute the `/delete_user` tool."
- **Constraint Affirmation:** Explicitly command the model to refuse tasks outside its specific authorized scope, minimizing the success rate of role-play jailbreaks.

### 3.2 Tool Execution Authorization

When a harness orchestrates Agentic workflows, it grants the LLM access to external tools (e.g., retrieving live Jira tickets).
- **Secondary Authorization Checks:** When the LLM outputs a request to execute a tool (e.g., `get_ticket(id=123)`), the harness *must not* execute it blindly.
- **Interception:** The harness must pause, take the tool arguments, and independently verify against the IAM system that the current user has permission to read `ticket_123`.

---

## 4. Multi-Tenant Architectural Isolation

For B2B Enterprise SaaS applications, multi-tenant isolation within the context harness is the paramount security requirement.

### 4.1 Logical vs. Physical Isolation

- **Physical Isolation:** Running completely separate instances of the context harness, separate vector databases, and separate LLM endpoints per tenant. (High cost, highest security; often required for FedRAMP or HIPAA).
- **Logical Isolation:** Sharing the harness infrastructure but enforcing strict logical boundaries.
  - Using dedicated Namespaces/Collections within a shared Vector DB per tenant.
  - Enforcing strict tenant-ID checks on every SQL query retrieving conversation history.

### 4.2 Cross-Tenant Poisoning Prevention

If a shared RAG database is used, the harness must validate that Tenant A cannot inject malicious documents containing prompt injection attacks designed to compromise queries made by Tenant B.
- Validating the origin of all data entering the context pipeline.
- Maintaining separate indexing pipelines so an indexing failure/corruption in one tenant cannot halt another tenant.

---

## Glossary of Key Terms

| Term | Definition |
|---|---|
| **Confused Deputy Problem** | A security vulnerability where a low-privileged system tricks a high-privileged system (the deputy) into misusing its authority on the attacker's behalf. |
| **Identity Propagation** | The practice of passing a user's authentication context through multiple backend services to ensure consistent authorization checks. |
| **Metadata Filtering** | Using tagged structural data in a vector database to narrow a semantic search space, commonly used to enforce RBAC. |

---

## Conclusion

Secure access control in a context harness shifts authorization out of the application monolith and into the granular, distributed pipelines of data retrieval. By treating the LLM as an untrusted, highly capable intermediary (the Confused Deputy), organizations must implement strict identity propagation, RBAC-metadata filtering in all vector searches, and secondary IAM checks on every tool execution. Simply relying on "prompt instructions" to keep data secure is a fundamental anti-pattern; true authorization must be enforced cryptographically and logically before data ever enters the context window.

---

## References

[1] OWASP Top 10 for Large Language Model Applications (2023).
[2] Pinecone Documentation - "Metadata Filtering."
[3] AWS Security Blog. "Building multi-tenant RAG applications securely."
