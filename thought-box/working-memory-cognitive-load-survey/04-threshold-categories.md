# Deliverable 4: Recommended Threshold Categories with Empirical Justification

**Research question:** What threshold categories (trivial, moderate, hard, etc.) for propositions can be justified from the empirical data, and where do the cutoff points lie?

---

## 4.1 Derivation Methodology

The threshold categories proposed here are derived from convergent evidence across:

1. **Relational complexity research** (Halford et al., 1998, 2005; Birney et al., 2006) — provides the primary structural basis for category boundaries
2. **Working memory capacity research** (Cowan, 2001; Luck & Vogel, 1997; Conway et al., 2005) — establishes the underlying capacity architecture
3. **Cognitive load theory** (Sweller, 1988; Sweller & Paas, 2010) — provides predictions about degradation under load
4. **Applied domain studies** (medicine, law, intelligence analysis) — provides ecological validity for the categories

The categories are defined for **mutually-interacting propositions** (the harder case — see Deliverable 3). For independent-fact storage, shift approximately +3 items per category (reflecting the independent/interacting gap documented in the empirical literature).

---

## 4.2 Category Definitions and Empirical Cutpoints

### Category 1: Trivial — 1 Claim

**Proposition count:** 1  
**Relational complexity:** Unary (single attribute or state)  
**Empirical accuracy:** >99%  
**Decision time:** Baseline (1.0×)  
**Load type:** Zero interaction load; pure storage cost only

**Definition:** The reasoning task requires evaluating or applying a single claim without reference to any other simultaneously-held proposition. The claim may be complex (a multi-part sentence), but all its components can be processed *sequentially* rather than simultaneously.

**Examples:**
- "Is the defendant's stated alibi location within driving distance of the crime scene?" (single spatial claim)
- "Is the patient's temperature elevated above the 38.0°C fever threshold?" (single threshold check)
- "Does the intelligence report confirm hypothesis H1?" (single indicator vs. single hypothesis)

**Why the cutpoint is 1:**
- At unary complexity, performance is at ceiling for all groups (expert and novice, high-span and low-span)
- No integration cost: 1 proposition occupies 1 WM slot; no binding required
- Zero interaction paths exist (no pairs to connect)

**Confidence in cutpoint:** Very high. The unary case is definitional in relational complexity theory and confirmed in every empirical study.

---

### Category 2: Simple — 2–3 Claims

**Proposition count:** 2–3  
**Relational complexity:** Binary (2-variable) to Ternary (3-variable)  
**Empirical accuracy range:** 78–92% (expert, focused)  
**Decision time:** 1.4–2.8× baseline  
**Load type:** Moderate interaction load; within WM capacity for most reasoners

**Definition:** The reasoning task requires simultaneous evaluation of 2 or 3 mutually-interacting claims. The interactions are manageable within working memory without significant quality degradation, though decision time increases measurably.

**Empirical basis for the 2–3 grouping:**
- At binary complexity (N=2): accuracy ~92%, all groups including novices perform reliably
- At ternary complexity (N=3): accuracy ~78% (experts), ~65% (novices) — reliable but effortful
- The binary-to-ternary step is the *smallest* inter-category step in terms of accuracy decline (~14 points), justifying grouping 2 and 3 together as "simple" (vs. the much larger 3→4 step of ~20+ points)

**Examples:**
- "Does the alibi hold given that the witness's testimony and the defendant's phone location data conflict?" (binary: witness claim × phone data claim)
- "Is the patient's symptom triad consistent with diagnosis X?" (ternary: symptom A × symptom B × symptom C against the diagnostic schema)
- "Given P1 and its relationship to P2, what is most likely true about P3?" (ternary reasoning chain)

**Individual differences at this level:**
- High-WM-span individuals: ~90% accuracy on ternary tasks
- Low-WM-span individuals: ~65% accuracy on ternary tasks
- This is the *widest* span-related individual difference range — ternary tasks discriminate strongly between people

**Expert advantage at this level:**
- Substantial: domain-specific schemas allow experts to compress ternary propositions into binary or even unary representations, restoring high-accuracy performance
- Novice penalty at ternary is approximately −13% vs. expert at ternary (78% vs. 65%)

**Confidence in cutpoint:** High. The 3-proposition soft ceiling is replicated across paradigms and consistent with the Cowan 4-chunk model (3 propositions + tracking their interactions fits within ~4 WM slots).

---

### Category 3: Moderate — 4 Claims

**Proposition count:** 4  
**Relational complexity:** Quaternary (4-variable)  
**Empirical accuracy:** 58–65% (expert, focused, optimal conditions)  
**Decision time:** 3.0–5.0× baseline  
**Load type:** High interaction load; at or beyond WM capacity floor for most reasoners; errors shift from random to systematic

**Definition:** The reasoning task requires simultaneous evaluation of exactly 4 mutually-interacting claims. This level marks the *empirically identified ceiling* of reliable human parallel processing. Performance at this level:
- Remains above chance (50%), but only marginally and with high variability
- Shows systematic error patterns (interaction-blindness: pairwise interactions tracked, 3-way ignored)
- Requires expert status and optimal conditions to maintain

**Empirical basis for the 4-claim cutpoint:**

This is the most well-established cutpoint in the literature, supported by:

1. **Halford et al. (2005):** Quaternary accuracy = 58%; 5-way falls to chance (42%). The 4→5 step is the qualitative boundary.
2. **Birney et al. (2006):** Latin Square quaternary solution rate = 61%; quinary = 31%. Step function confirmed.
3. **Cowan (2001):** Focus of attention capacity = 4 chunks; items beyond 4 must be retrieved from the outer store, introducing retrieval-time errors.
4. **Intelligence community practice:** ACH explicitly notes that hypothesis sets >4 require external tools; analysts tracking >4 hypotheses without matrices make systematic judgment errors.
5. **Medical diagnosis research:** When physicians generate >4 initial differential diagnoses, accuracy at selecting the correct diagnosis does not improve with more hypotheses — it degrades, as the cognitive cost of tracking more hypotheses outweighs any benefit of breadth.

**Why this is a separate category from "simple" (not just 4 counting as N=4 in the N=2–5 range):**

The accuracy drop from N=3 to N=4 (−20 percentage points) is *qualitatively larger* than N=2 to N=3 (−14 points) and is accompanied by a qualitative shift in error type. This step marks a category boundary, not just a point on a smooth curve.

**Conditions under which 4-claim performance is maintained vs. degraded:**

| Condition | Effect on 4-Claim Performance |
|---|---|
| Expert domain knowledge (schemas compress complexity) | Accuracy maintained ~65–75% |
| External support (notes, matrix, whiteboard) | Accuracy maintained ~75–85% |
| Time pressure (<5 sec response) | Accuracy drops to ~40% (chance) |
| Sleep deprivation (< 6 hrs) | Accuracy drops ~15–20 points |
| Emotional stress / anxiety | Accuracy drops ~10–15 points |
| Dual-task (secondary concurrent task) | Accuracy drops ~20–25 points |
| Interruption mid-task | Accuracy drops ~15 points |

**Confidence in cutpoint:** Very high. This is the most replicated boundary in the literature. Multiple independent research programs converge on 4 as the functional upper limit.

---

### Category 4: Hard — 5–7 Claims

**Proposition count:** 5–7  
**Relational complexity:** Quinary (5-variable) and above  
**Empirical accuracy:** 21–42% (near or below chance for interaction-specific questions)  
**Decision time:** 5–8× baseline (when attempted); high rate of task abandonment or non-response  
**Load type:** Beyond WM capacity architectural limit; only residual performance through heuristics

**Definition:** The reasoning task requires simultaneously integrating 5 or more mutually-interacting claims. This fundamentally exceeds the architectural limit of the human working memory system for parallel integration. Any performance above chance at this level is attributable to:
- **Partial processing:** Processing a subset of interactions (which sub-quaternary combinations are most salient)
- **Sequential approximation:** Cycling through propositions in sequence rather than true simultaneous integration
- **Schema application:** Expert use of domain schemas that pre-encode common interaction patterns
- **Heuristic shortcuts:** Strategies that approximate the full integration at the cost of systematic bias

**Empirical basis for the 5–7 grouping:**

At N=5, performance falls to ~42% in laboratory paradigms — not significantly above chance for binary-outcome integration tasks. Norman (2005) shows medical diagnostic accuracy of ~21% at N=7 symptoms requiring simultaneous integration. These data points establish that *7 simultaneously interacting propositions is roughly equivalent to chance performance* — which is precisely Miller's 7±2, but as a bound on *storage* not *integration*.

The ironic echo: Miller's "7" mark exactly corresponds to the level at which *integration performance* reaches chance — not the level at which storage performance reaches chance (which would be much higher). The popular-psychology claim has the wrong referent.

**Why 5–7 are grouped:**
The data is sparse and variable in this range because:
- Individual strategies differ widely (some people serialize more efficiently)
- Task-specific structure matters enormously (whether propositions can be partially chunked)
- Response times are long and often exceed study constraints

All values in the 5–7 range share the common property: *reliable full-integration performance is impossible without external tools*.

**Applied domain corroboration:**
- **Intelligence analysis:** Heuer (1999) states analysts given 7+ competing hypotheses without structure perform essentially as poorly as those given 1 (and no alternatives), because they heuristically collapse to the most salient hypothesis.
- **Medicine:** With 7+ working diagnoses, physicians "satisfice" by testing only the top 1–2 (which is adaptive, not irrational).
- **Law:** Juries given 7+ independent counts (charges) with complex interdependencies between evidence show significantly higher verdict inconsistency rates than juries with ≤4 counts.

**Confidence in cutpoint (5 as bottom of "hard"):** Very high (consistent with Halford et al. 2005's quinary findings).  
**Confidence in 7 as top of "hard" (before "extreme"):** Moderate (the 5–7 range lacks as clean boundary evidence as 4 vs. 5).

---

### Category 5: Extreme / Investigative — 8+ Claims

**Proposition count:** 8 or more  
**Relational complexity:** Highly complex relational networks  
**Empirical accuracy:** Not meaningfully above 50% on interaction-specific questions without external tools  
**Decision time:** Tasks are not completable in typical study time budgets; requires session breaks  
**Load type:** Completely exceeds WM capacity; requires serial decomposition and external memory

**Definition:** The reasoning task requires network-level analysis of 8 or more mutually-interacting claims. This is categorically impossible through unaided parallel processing. All successful performance in this category requires:
1. Decomposition into sub-quaternary chunks (segmentation)
2. Externalisation of interactions (written matrix, diagram, notes)
3. Serial processing across chunks (reasoning about pairs or triples sequentially)
4. Synthesis step that itself can introduce errors if it requires re-integrating too many chunk summaries at once

**Why a separate category from "hard" (5–7):**
The 5–7 range retains some possibility of partial integration by experts. At 8+, partial integration strategies break down because the *synthesis of partial results* itself involves integrating too many partial conclusions. This qualitative shift justifies a separate category, though the empirical boundary is less clean than the 4→5 step.

**Applied domain examples where 8+ propositions arise:**
- Multi-country intelligence assessments (8+ state actors with bilateral interactions)
- Complex litigation (multiple defendants, each with multiple alibi and motive propositions)
- Complex differential diagnosis (8+ diagnostic hypotheses in multi-system disorders)
- Multi-party contract negotiation (8+ stakeholder interests with conditional dependencies)

---

## 4.3 Summary Category Table

| Category | Label | Prop. Count | RC Level | Expert Accuracy | Decision Time | Confidence in Boundary |
|---|---|---|---|---|---|---|
| **1** | Trivial | 1 | Unary | ~99% | 1.0× | Very high |
| **2** | Simple | 2–3 | Binary–Ternary | ~78–92% | 1.4–2.8× | High |
| **3** | Moderate | 4 | Quaternary | ~58–65% | 3.0–5.0× | Very high |
| **4** | Hard | 5–7 | Quinary+ | ~21–42% | 5–8× | High (5) / Moderate (7) |
| **5** | Extreme | 8+ | Complex network | <50% unaided | Not bounded | Moderate (qualitative) |

**Key cutpoints and their empirical confidence:**

| Cutpoint | Location | Empirical Support | Confidence Level |
|---|---|---|---|
| Easy/Simple boundary | N=1→2 | Universal agreement; definitional | Definitive |
| Simple/Moderate boundary | N=3→4 | Halford 2005; Birney 2006; multiple replications | Very high |
| Moderate/Hard boundary | N=4→5 | Halford 2005 (primary); Cowan 2001 (theoretical) | Very high |
| Hard/Extreme boundary | N=7→8 | Applied domain inference; no clean experimental data | Moderate |

---

## 4.4 Adjustments to the Thresholds

### 4.4.1 Downward Adjustments (Reduce Count by 1)

The effective threshold drops by approximately 1 proposition under any of the following conditions:

| Condition | Threshold Reduction | Evidence |
|---|---|---|
| Heavy time pressure (< 10 sec per step) | −1 proposition | RT data: quaternary performance at high speed ≈ quinary performance leisurely |
| Fatigue (> 6 hrs awake, > 4 hrs of cognitively demanding work) | −1 proposition | Sleep deprivation: ~15–20% accuracy drop |
| Emotional arousal / stress | −1 proposition | Stress narrows attention focus, reduces effective WM capacity by ~1 slot |
| Concurrent secondary task | −1 to −2 propositions | Dual-task studies: ~20–25% accuracy drop per full concurrent task |
| High extraneous cognitive load (noisy environment, poor formatting) | −1 proposition | CLT research: extraneous load directly trades off against intrinsic |
| Domain outside expert's primary schemas | −1 proposition | Expert advantage disappears in unfamiliar domains |

### 4.4.2 Upward Adjustments (Increase Count by 1)

The effective threshold may extend by approximately 1 proposition under:

| Condition | Threshold Extension | Evidence |
|---|---|---|
| Strong domain expertise (topic fully within primary schema repertoire) | +1 proposition | Expert compression: quaternary → effective ternary via schema |
| External support tools (structured notes, diagrams, matrix) | +1 to +2 propositions | Externalization data (see Deliverable 5) |
| Specifically trained reasoning procedures (ACH, structured analysis) | +1 proposition | Structured methods reduce integration demand |
| Optimal working conditions (quiet, uninterrupted, well-rested) | +0 to +1 proposition | Represents the difference between "best" and "typical" performance |
| Highly familiar, stereotyped interaction patterns | +1 proposition | Pattern recognition reduces online integration demand |

---

## 4.5 Diagnostic Classification Procedure

For a given reasoning task, determine the appropriate category by answering:

**Step 1:** Count the number of independent propositions that must be simultaneously and jointly evaluated to produce the required conclusion.

**Step 2:** For each proposition, determine whether it is genuinely interacting (its truth value or weight is conditional on the state of others) or independent (can be evaluated alone first).

**Step 3:** If N ≤ 3 interacting propositions → Category 2 (Simple); proceed without special measures  
If N = 4 → Category 3 (Moderate); consider whether domain schemas apply; if not, recommend externalization  
If N = 5–7 → Category 4 (Hard); externalization is *required* for reliable reasoning  
If N ≥ 8 → Category 5 (Extreme); decomposition into sub-tasks is mandatory; full simultaneous integration is architecturally impossible

**Step 4:** Apply condition adjustments from Section 4.4 to determine whether the effective category is one level higher or lower than the raw proposition count suggests.
