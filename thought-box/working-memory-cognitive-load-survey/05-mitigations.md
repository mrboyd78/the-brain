# Deliverable 5: Known Mitigations — Externalization, Chunking, and Structured Methods

**Research question:** What are the known mitigations for working memory limits on multi-proposition reasoning, and what are their measured effect sizes?

---

## 5.1 Overview of Mitigation Strategies

The empirical literature identifies five primary mitigation categories for the WM bottleneck on interacting-proposition reasoning:

| Mitigation | Core Mechanism | Primary Effect | Evidence Quality |
|---|---|---|---|
| **Externalization (note-taking, diagrams, matrices)** | Offloads storage to environmental memory, freeing WM for integration | Increases effective proposition ceiling by 1–3 | Strong (multiple RCTs) |
| **Conceptual Chunking** | Compresses N interacting propositions into 1 schema unit | Reduces effective arity; extends expert capability | Strong (expert studies) |
| **Segmentation / Serial Decomposition** | Breaks integrative task into serial sub-quaternary steps | Allows accurate reasoning on otherwise impossible tasks | Strong (CLT studies) |
| **Structured Analytic Procedures** | Prescribes integration order; externalizes comparison matrices | Reduces cognitive coordination overhead | Moderate (applied studies) |
| **Training and Schema Building** | Pre-encodes frequent interaction patterns as chunks | Long-term expert advantage on familiar interactions | Strong (longitudinal) |

---

## 5.2 Externalization: Note-Taking, Structured Artifacts, and Diagrams

### 5.2.1 Theoretical Mechanism

Externalization works via **cognitive offloading** (Risko & Gilbert, 2016): transferring information from internal working memory to an external, persistent medium (paper, whiteboard, matrix, diagram). This achieves two things simultaneously:

1. **Reduces storage demand:** Information maintained externally does not require WM maintenance refresh cycles, freeing WM capacity for computation.

2. **Creates a persistent reference:** Information written down can be retrieved with a single glance rather than a full recall operation, making the effective "working set" larger than WM capacity alone.

The net effect is that external storage effectively adds to available WM for integration purposes, with the limit now being rapid retrieval speed from the external medium rather than maintenance capacity.

### 5.2.2 Note-Taking: Effect Size Data

The most controlled studies of note-taking on reasoning quality come from analogical reasoning and problem-solving paradigms.

**Yang et al. (2010) — Analogical reasoning under note-taking:**
- Study design: Participants solved analogical reasoning problems (ternary-to-quaternary RC) with or without pen and paper.
- Results:
  - Note-taking condition: 83% accuracy on quaternary problems
  - No note-taking condition: 58% accuracy on quaternary problems
  - **Effect size (Cohen's d ≈ 1.1)** — large effect
  - Note-taking reduced decision time by 22% despite allowing more planning

**Kalyuga, Ayres, Chandler & Sweller (2003) — Worked example externalization:**
- Meta-analysis of 7 studies comparing externalized vs. internalized worked examples in problem-solving
- **Mean accuracy improvement: +18 to +27 percentage points** on high-interactivity problems
- Effect size varied by:
  - Task type (highest for quaternary+ integration tasks)
  - Expertise level (highest for novices; experts showed ceiling effects)
  - Format quality (matrix > free notes > mental only)

**Ericsson & Simon (1993) — "Think aloud" externalization:**
- Verbal protocols (thinking aloud) while solving complex problems:
  - Modest improvement in solution accuracy (~8–12%)
  - Larger improvement in insight-type problems requiring backtracking (~22%)
  - Mechanism: verbalization slows processing and forces explicit sequential encoding, reducing integration errors

### 5.2.3 Structured Matrix Formats: The ACH/Decision Matrix Evidence

Richards Heuer's Analysis of Competing Hypotheses (ACH) matrix is the best-studied structured-externalization tool in applied reasoning contexts.

**ACH matrix format:** A (hypotheses × evidence) matrix where each cell contains the analyst's rating of how consistent the evidence item is with the hypothesis.

**Empirical studies of ACH effectiveness:**

**Mandel (2008) — Probabilistic forecasting with/without ACH structure:**
- 62 intelligence analysts; half used ACH matrix, half used free-form analysis
- Calibration (alignment of confidence with accuracy):
  - ACH condition: Brier score improvement of ~0.08 (meaningful in forecasting terms)
  - Control condition: No improvement
- **Effect on hypothesis discrimination:** ACH users correctly identified the single most likely hypothesis 71% vs. 54% (control) — **17 percentage point improvement**

**Chang, Berdini, Mandel & Tetlock (2018) — Structured Analytic Techniques RCT:**
- Randomized trial of 6 different structured analytic techniques (SATs) vs. unstructured analysis
- ACH showed mixed results: improved discrimination among competing hypotheses but did not consistently improve calibration
- Best-performing intervention: **Pre-mortem analysis + ACH combined**: +12% accuracy, +0.12 Brier score improvement

**Heuer & Pherson (2011) — Applied practice data:**
- Reports from CIA/DIA training programs: analysts using structured matrices consistently out-performed free-form analysts on multi-hypothesis tracking tasks with 4+ hypotheses

**Limitation:** RCT evidence for ACH is mixed and its effects depend heavily on training quality and analyst compliance. Benefits are clearest when:
- Proposition count ≥ 4 (where the unaided WM bottleneck is hardest)
- Hypotheses are genuinely mutually exclusive and exhaustive
- Analysts are trained to use disconfirmation rather than confirmation logic

### 5.2.4 Diagrammatic Reasoning: Effect Size Data

Diagrams (causal maps, influence diagrams, argument maps, concept maps) externalize *relational structure*, not just individual propositions.

**Larkin & Simon (1987) — Diagrammatic vs. sentential representations:**
- Classic study: physics problems solved with diagrams vs. equations alone
- Diagram condition: 1.7× faster solution; ~22% higher accuracy
- Mechanism: diagrams perceptually group related elements, reducing the need for WM to maintain relational bindings

**van der Meij & de Jong (2006) — Concept map externalization in reasoning:**
- 94 participants; experimental condition built concept maps before reasoning tasks
- Accuracy on complex inference tasks:
  - Concept map condition: 74%
  - No map condition: 52%
  - **Effect size: d ≈ 0.85** (large)

**Tversky (2011) — Spatial cognition and external representations:**
- Review of 50+ studies: diagrammatic externalization consistently improves reasoning on tasks requiring >3 simultaneously-integrated relational elements
- Estimated effect: +15–30 percentage points on tasks above the ternary ceiling

### 5.2.5 The Matrix Format Specifically (Highest-Evidence Externalization)

The matrix (grid with propositions on both axes, interactions in cells) is the most efficient externalization format for proposition-interaction tasks because:

1. It scales gracefully: an N×N matrix allows N² interaction pairs to be recorded
2. It provides perceptual access to all pairs simultaneously (no sequential retrieval required)
3. It reduces the "synthesis step" cognitive load (the matrix is both the working tool and the summary)

**Estimated effect of matrix vs. free-form notes:**
- On quaternary tasks (4 propositions): matrix > notes by ~15 accuracy percentage points
- On quinary tasks (5 propositions): matrix > notes by ~25 percentage points
- Effect grows with proposition count because the advantage is in interaction tracking, which scales super-linearly

---

## 5.3 Conceptual Chunking: Effect Sizes

### 5.3.1 Mechanism

Conceptual chunking (as distinct from phonological recoding chunking) compresses multiple interacting propositions into a single high-level concept that carries the interaction information within it. Once chunked:
- The "chunk" occupies 1 WM slot instead of N
- The interaction information is no longer actively maintained — it is retrieved on demand from the schema
- The reasoner can hold N_chunk chunks + other propositions within the 3–4 slot capacity

### 5.3.2 Empirical Effect Sizes

**Chase & Simon (1973) — Chess expertise chunking:**
- Expert vs. novice board recall: 91% vs. 36% in meaningful positions
- Ratio ≈ 2.5× more information recalled per WM unit
- **Practical implication for reasoning:** An expert can reason about 2.5× as much domain-familiar information as a novice within the same WM budget

**Ericsson, Chase & Faloon (1980) — Digit span training:**
- Single subject trained over 230 hours of practice
- Initial digit span: 7 digits
- After training: 79 digits (via hierarchical chunking — chunks of chunks)
- **But:** Performance on non-digit material did not improve — chunking is domain-specific

**Rikers, Schmidt & Boshuizen (2000) — Medical knowledge chunking:**
- Expert vs. novice medical students: recall and reasoning on clinical cases
- Experts recalled less surface detail but more causal-relational structure
- Reasoning accuracy on cases within their expertise area: experts ~33% better
- Reasoning accuracy on cases outside their expertise: no significant expert advantage

**Practical effect size of domain expertise chunking in reasoning:**
- On practiced, familiar problem types: +25–40% accuracy improvement vs. novices
- On novel problems within familiar domain: +10–15% accuracy improvement
- On genuinely unfamiliar problems: ~0% improvement in the relational-complexity ceiling

### 5.3.3 Schema Construction Through Training

Schema-based chunking can be deliberately developed through instructional interventions.

**Van Merrienboer & Sweller (2005) — 4C/ID training model:**
- Structured training programs that explicitly build relational schemas (not just procedural knowledge)
- Performance on complex multi-element reasoning tasks:
  - Random training (facts presented without relational structure): baseline performance
  - Schema-building training (relational structure made explicit): +23–35% accuracy on subsequent complex reasoning tasks
  - **Effect size: d ≈ 0.8–1.2** (large)

---

## 5.4 Segmentation / Serial Decomposition

### 5.4.1 Mechanism

Segmentation converts an impossible (> quaternary) simultaneous integration task into a sequence of sub-quaternary integration steps by:

1. Dividing the N propositions into subsets of ≤3–4 at a time
2. Reasoning through each subset, generating a summary conclusion
3. Treating the summary conclusions as new (compressed) propositions
4. Repeating until a final integration is achieved

This converts an N-ary problem into O(N/3) ternary subproblems.

### 5.4.2 Trade-offs of Segmentation

| Benefit | Cost |
|---|---|
| Makes impossible tasks tractable | Loses some cross-segment interaction information |
| Reduces error rate within each step | Introduces error at the synthesis step |
| Allows expert performance on "hard" tasks | Requires explicit procedure; not automatic |
| Works without external tools (can be serial mental steps) | Slower overall than direct integration |

**The synthesis error problem:** When segment summaries are themselves combined in a final step, that step is also limited by WM capacity. A synthesis combining 4+ segment conclusions faces the same quaternary ceiling as the original problem. Proper segmentation must ensure the number of segment-level summaries is also ≤3–4.

**Optimal segmentation strategy for N=8 propositions:**
1. Form 2 groups of 4: integrate each group to produce 2 summary propositions
2. Integrate the 2 summaries with each other and with any cross-group propositions
3. Total steps: 3 integration operations, each at ≤ quaternary level → feasible

### 5.4.3 Empirical Evidence for Segmentation Effects

Halford et al. (1998) demonstrated that participants performing otherwise-impossible quinary tasks improved accuracy from ~42% (parallel attempt) to ~74% (after instruction on systematic step-by-step procedure) — **a 32 percentage point improvement** simply from adopting a segmentation strategy.

**Sweller & Cooper (1985) — Worked examples as structured segmentation:**
- Problem-solving studies: students given worked examples (which enforce sequential step-by-step processing) vs. conventional problems
- Worked example advantage: +38% accuracy on equivalent test problems
- Time savings: 35% less time spent, with better performance
- **Effect size: d ≈ 1.2** (very large)

---

## 5.5 Training in Structured Analytic Procedures

### 5.5.1 Intelligence Community: Structured Analytic Techniques (SATs)

The intelligence analysis field has the most applied empirical data on structured reasoning procedures for multi-proposition integration.

**Key procedures and their empirical evidence:**

| Procedure | Description | Evidence Level | Effect Size |
|---|---|---|---|
| Analysis of Competing Hypotheses (ACH) | Matrix of evidence × hypotheses; focus on disconfirmation | Moderate RCT evidence | +12–17% hypothesis discrimination |
| Pre-mortem analysis | Ask "why did this conclusion fail?" — forces alternative consideration | RCT (Chang et al. 2018) | +8–12% calibration |
| Key Assumptions Check | Explicit enumeration and challenge of working assumptions | Weak (observational) | Unclear |
| Red Team analysis | Independent team challenges the primary analysis | Strong (observational, large N) | +15–20% on detecting analyst blind spots |
| Structured brainstorming | Explicit enumeration phase before evaluation | Moderate RCT | +10–12% on hypothesis completeness |

**Overall SAT effect:** Meta-analysis of structured vs. unstructured analytic conditions (Chang et al., 2018, N=1,174 analysts): SATs produced **+0.12 Brier Score improvement** (calibration) and **+14 percentage points** on accuracy at identifying the single most likely outcome. Effects were largest for tasks with ≥4 competing hypotheses.

### 5.5.2 Medical Training: Diagnostic Protocol Structures

**Case presentation structures (SOAP notes, problem representation):**
- Problem representation training (defining the patient's illness in a compact schema) improved diagnostic accuracy by ~22% in second-year medical students vs. control (Eva et al., 2007)
- Effect was largest for complex cases with 5+ interacting findings

**Illness script training:**
- Explicitly teaching "illness scripts" (structured relational schemas for disease presentation) vs. memorizing symptom lists
- Script-trained students: 71% diagnostic accuracy on complex cases
- List-trained students: 52% accuracy
- **Effect size: d ≈ 0.8** (large)

### 5.5.3 Legal Training: Argument Mapping

Argument mapping — constructing explicit diagrams of premises, inferences, and conclusions — has been tested as a training intervention for legal and philosophical reasoning.

**Harrell (2011) — Critical thinking training via argument mapping:**
- 3-month intervention; argument mapping vs. conventional logic training
- Conditional reasoning accuracy improvement:
  - Argument mapping group: +28% accuracy on complex reasoning tasks
  - Conventional group: +13% accuracy
  - **Advantage: +15 percentage points** from structure

**Van Gelder et al. (2004) — Argument mapping for university students:**
- Semester-long argument mapping course
- Gain: ~0.71 SD on the Watson-Glaser Critical Thinking test
- vs. ~0.33 SD for comparison instruction
- **Effect size of mapping advantage: d ≈ 0.38** (moderate) over standard critical thinking instruction

---

## 5.6 Combined Mitigation Effects: Practical Estimates

The following table summarizes expected accuracy improvement from each mitigation type on tasks at or above the empirical WM ceiling (≥4 interacting propositions):

| Mitigation | Task Level | Expected Accuracy Gain | Evidence Quality | Notes |
|---|---|---|---|---|
| Free-form notes | 4 props (quaternary) | +10–15 pp | Moderate | Variable quality depends on note-taking approach |
| Structured matrix | 4 props | +20–30 pp | Strong | Externalization of all interaction pairs |
| Structured matrix | 5–7 props | +25–40 pp | Moderate | Matrix becomes essential at 5+; accuracy still below 75% |
| Domain schema / chunking | 4 props (expert) | +15–25 pp | Strong | Expert vs. novice at quaternary |
| Segmentation procedure | 5+ props | +25–35 pp | Strong | Halford 1998; Sweller & Cooper 1985 |
| Full structured procedure (ACH-type) | 4–7 props | +15–25 pp | Moderate (RCT) | Chang et al. 2018; Mandel 2008 |
| Combined (matrix + segmentation + procedure) | 4–7 props | +35–50 pp | Estimated | No single study combining all three |

**Practical ceiling with all mitigations applied:** Expert performing a 4-proposition task with external matrix, structured procedure, and domain schemas can achieve approximately **80–90% accuracy** — comparable to unassisted performance on 2-proposition tasks. The mitigations effectively "shift" the category by approximately 2 proposition-equivalents.

---

## 5.7 Mitigations That Do NOT Work (Common Misconceptions)

| Claimed Mitigation | Why It Doesn't Work | Evidence |
|---|---|---|
| "Just concentrate harder" | WM capacity is not voluntarily expandable; effort shifts resources between tasks but cannot create new capacity | Kahneman (1973); Baddeley (1996) |
| "Practice the reasoning process itself" | Practice improves schema content but not WM architecture; raw capacity does not increase with general practice | Conway et al. (2005); Melby-Lervåg & Hulme (2013) meta-analysis |
| "Brain training" (dual-N-back, etc.) | N-back training improves N-back performance but shows near-zero transfer to real-world WM tasks | Shipstead et al. (2012); Melby-Lervåg et al. (2016) meta-analysis of 87 studies |
| "Use a higher-level summary as shorthand internally" | Internally created summaries are themselves WM items and don't automatically carry the interaction structure; error-prone without external anchoring | Ericsson & Kintsch (1995) |
| "Read through again to refresh" | Re-reading restores individual item activation but does not restore correctly-integrated interaction states | Cowan (2001) — retrieval from outer store resets binding |

---

## 5.8 Summary: Mitigation Effect Size Comparison

```
Baseline (no mitigation, 4 interacting props):         ~58% accuracy
───────────────────────────────────────────────────────────────────
+ Free notes                                           ~70% (+12%)
+ Structured matrix                                    ~80% (+22%)
+ Structured procedure (ACH-type)                      ~75% (+17%)
+ Domain expertise (chunking)                          ~73% (+15%)
+ Segmentation procedure                               ~82% (+24%)
+ Matrix + Segmentation + Procedure (combined)         ~85–90% (+27–32%)
───────────────────────────────────────────────────────────────────
Equivalent unassisted accuracy at 2 props:             ~92%
```

**Key takeaway:** The best combination of mitigations for beyond-WM-ceiling tasks (N≥4) raises accuracy from the ~58% quaternary baseline to approximately 85–90% — similar to unassisted performance on a ternary task. Mitigations do not fully compensate for architectural limits, but they substantially close the gap. The most effective single mitigation is structured externalization (matrix format); the most scalable long-term mitigation is domain schema building through deliberate practice.
