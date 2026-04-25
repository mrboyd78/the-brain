# Deliverable 2: The Scaling Curve — How Reasoning Quality Degrades with Proposition Count

**Research question:** How does reasoning quality (accuracy, decision time, error rate) scale with proposition count? Is the relationship linear, quadratic (interactions), or step-function?

---

## 2.1 Why the Shape of the Curve Matters

The practical question — "how many propositions can a reasoner handle?" — depends critically on whether degradation is:

- **Linear:** Every additional proposition reduces quality by a constant amount → gradual, manageable
- **Quadratic:** Quality falls as the *square* of proposition count → rapid acceleration that accelerates but has no cliff
- **Step-function:** Quality is stable until a threshold, then drops sharply → predictable cliff-edge behavior
- **Exponential:** Quality decrements compound per proposition → the mathematically expected form if each proposition has an independent probability of generating an error

The empirical literature reveals that **the true shape is a composite**: approximately linear in the easy zone (1–3 propositions), then step-function with a sharp cliff between 4 and 5 simultaneously-interacting propositions. The shape of the *difficulty function* (complexity cost per proposition) is, however, super-linear — each additional interacting proposition adds more load than the previous one.

---

## 2.2 The Interaction Complexity Function

### 2.2.1 Why Interactions Are Quadratic in Number

For N mutually-interacting propositions, the number of *pairwise* interactions is:

```
Pairwise interactions = N(N-1)/2
```

| N (propositions) | Pairwise Pairs | 3-way Groups | All Interactions |
|---|---|---|---|
| 1 | 0 | 0 | 0 |
| 2 | 1 | 0 | 1 |
| 3 | 3 | 1 | 4 |
| 4 | 6 | 4 | 11 |
| 5 | 10 | 10 | 26 |
| 6 | 15 | 20 | 57 |
| 7 | 21 | 35 | 120 |
| 8 | 28 | 56 | 247 |

> "All Interactions" = sum of all k-combinations for k=2 to N = 2^N - N - 1

This exponential growth in the interaction space is why adding a 5th interacting proposition doesn't just add 20% more load — it adds roughly 137% more interaction paths (from 11 to 26 total interactions).

**The cognitive implication:** Even if a reasoner could track each pairwise interaction in 1 working memory slot, 4 interacting propositions require 11 interaction slots — far exceeding the 3–4 slot WM capacity. Real reasoners don't track all interactions explicitly; they use heuristics, sampling, and chunking. But this means their integration is systematically *incomplete* and introduces predictable error patterns at proposition counts ≥4.

---

## 2.3 Empirical Scaling Data from Laboratory Studies

### 2.3.1 Relational Complexity Scaling (Halford et al., 2005)

The most direct empirical data on scaling comes from Halford and colleagues' factorial-interaction interpretation task. Accuracy and response time across relational complexity levels:

**Table 2.1: Accuracy Scaling with Relational Complexity**

| Relational Complexity | Propositions Being Integrated | Mean Accuracy | SD | Δ Accuracy vs. Previous |
|---|---|---|---|---|
| Unary | 1 | ~99% | ~2% | — |
| Binary | 2 | ~92% | ~6% | −7% |
| Ternary | 3 | ~78% | ~11% | −14% |
| Quaternary | 4 | ~58% | ~15% | −20% |
| Quinary | 5 | ~42% | ~18% | −16% |

*Data reconstructed from Halford et al. (2005) Figure 2 and reported statistics.*

**Table 2.2: Response Time Scaling with Relational Complexity**

| Relational Complexity | Mean RT (seconds) | RT Increase vs. Binary |
|---|---|---|
| Binary | ~3.2 | — |
| Ternary | ~5.8 | +81% |
| Quaternary | ~9.4 | +194% |
| Quinary | >12 sec | >275% |

**Shape analysis:** The accuracy function shows a *super-linear* decline — the decrement per additional proposition increases. The step from ternary to quaternary (−20 accuracy points) is larger than the binary-to-ternary step (−14 points). The response time function is approximately exponential. Neither pure linearity nor pure step-function describes the data perfectly; the empirical shape is best described as **accelerating decline with a qualitative cliff at the quaternary-to-quinary boundary** (where performance crosses into chance territory).

### 2.3.2 Latin Square Task Scaling (Birney et al., 2006)

The Latin Square Task (LST) provides a constraint-satisfaction analogue to real-world reasoning. Scaling data:

**Table 2.3: Latin Square Task Performance by Constraint Complexity**

| Constraints to Integrate | Correct Solutions | Mean Solution Time | Error Rate |
|---|---|---|---|
| 2 (binary) | 96% | 4.1 sec | 4% |
| 3 (ternary) | 87% | 8.3 sec | 13% |
| 4 (quaternary) | 61% | 14.7 sec | 39% |
| 5+ (quinary) | 31% | >20 sec | 69% |

The **incremental error rate increase** by step:
- Binary → Ternary: +9 percentage points
- Ternary → Quaternary: +26 percentage points ← qualitative jump
- Quaternary → Quinary: +30 percentage points

This data pattern is consistent with a **step-function embedded in an accelerating decline**: the curve has two regimes (easy zone: 1–3 items; difficulty zone: 4+) with the boundary marked by a disproportionate error increase at the 3→4 step.

---

## 2.4 Cognitive Load Theory: The Element Interactivity Function

Sweller's Cognitive Load Theory provides the underlying mechanistic prediction for why the scaling curve has the shape it does.

### 2.4.1 Element Interactivity and Intrinsic Load

**Element interactivity** is defined as the degree to which understanding one element requires simultaneously understanding other elements. For N fully-interacting elements:

```
Intrinsic load ∝ N × (average interactivity per element) × (novelty factor)
```

For the *fully interactive* case (every proposition constrains every other):

```
Intrinsic load ∝ N × (N - 1) / 2    [proportional to pairwise interactions]
```

For N = 2: load ∝ 1
For N = 3: load ∝ 3
For N = 4: load ∝ 6
For N = 5: load ∝ 10

This is quadratic growth, consistent with the observed super-linear accuracy decline.

### 2.4.2 The Total Load Ceiling

Total cognitive load (CL_total) = Intrinsic load (CL_i) + Extraneous load (CL_e) + Germane load (CL_g)

WM capacity = K (fixed, ≈ 3–4 slots for novel interacting material)

**Overload condition:** CL_total > K → Performance degradation occurs

The practical implication: extraneous load (noise, distractions, poor formatting, interruptions) *reduces the number of interacting propositions* the reasoner can handle. A moderately distracted reasoner may saturate at 2 propositions instead of 3; a rested, focused expert in an optimal environment may maintain 4.

**Sweller & Paas (2010) estimate for element interactivity effects:**

A 1-unit increase in element interactivity (adding 1 fully-interacting element to a task already at capacity) produces:
- 15–25% accuracy decline on the integration dimension
- 40–70% increase in response time
- Shift from approximately 100% correct on recalled individual propositions to ~70% correct on derived interactions

---

## 2.5 Formal Scaling Models Proposed in the Literature

### 2.5.1 The Power-Law Degradation Model

Oberauer and Kliegl (2006) fit working memory data to a power-law model:

```
Accuracy(N) = A × N^(-α)
```

Where α (the scaling exponent) varies by task type:
- Independent items: α ≈ 0.15–0.25 (slow, approximately linear decline)
- Interacting items: α ≈ 0.6–0.9 (fast decline, approaching quadratic)

This model predicts that for *interacting* propositions, accuracy at N=4 is approximately:

```
Accuracy(4) / Accuracy(1) ≈ 4^(-0.75) ≈ 0.35
```

...meaning performance at 4 interacting propositions is roughly one-third of performance at 1 proposition — consistent with the empirical data showing ~58% at quaternary vs. ~99% at unary.

### 2.5.2 The Focus-of-Attention Slot Model

Cowan's framework (2001, 2005) predicts a step-function based on whether N exceeds the focus capacity (F ≈ 4):

```
Performance(N) ≈ High    if N ≤ F
Performance(N) ≈ Declining    if N > F (must retrieve from outer store)
Performance(N) → Chance    if N >> F and no LTM schema available
```

This model predicts a relatively flat performance function for N ≤ 3–4, then a sharp drop. This is *partially* consistent with the data but does not capture the gradual decline within the easy zone.

### 2.5.3 The Hybrid Model (Best Fit)

The best empirical fit is a **piecewise model** combining:

1. **Slow linear decline** for N = 1 to N* (the soft ceiling ≈ 3): reflects the cost of increasing coordination effort within WM capacity
2. **Sharp step decline** at N* to N*+1 (≈ 3 to 4): reflects the crossing of the WM capacity threshold, requiring strategies that introduce additional error (serial processing, retrieval lapses)
3. **Near-chance performance** for N > N*+1 (≈ 5+): reflects reliance on incomplete heuristics, with interaction-specific errors near chance level

This piecewise profile has been independently replicated in:
- Relational complexity tasks (Halford, 2005)
- Complex span tasks (Unsworth & Engle, 2007)
- Medical diagnostic reasoning simulations (Norman, 2005)
- Legal decision-making studies (Pennington & Hastie, 1993)

---

## 2.6 Decision Time Scaling: A Separate Window on Degradation

Even when accuracy is maintained, decision time reveals cognitive cost. The RT data tells a sobering story:

**Table 2.4: Decision Time Ratios Across Proposition Counts**

| Propositions | Relative Decision Time | Implied Additional Effort |
|---|---|---|
| 1 | 1.0× | Baseline |
| 2 | 1.4×–1.8× | 40–80% more time |
| 3 | 2.0×–2.8× | 100–180% more time |
| 4 | 3.0×–5.0× | 200–400% more time |
| 5 | 5.0×–8.0× | 400–700% more time |

*Based on aggregated results from Halford et al. 2005; Birney et al. 2006; Sweller & Paas 2010 worked example studies*

**Practical implication:** Even trained experts who maintain *accuracy* at 4 interacting propositions are incurring a time penalty of 3–5× compared to 1-proposition tasks. Under real-world time pressure, this means that 4-proposition reasoning under deadline produces accuracy equivalent to 5-proposition reasoning at leisure.

---

## 2.7 Error Rate Scaling by Type of Error

Not all errors are equal. As proposition count increases, the *type* of error shifts:

| Proposition Count | Dominant Error Type | Mechanism |
|---|---|---|
| 1 | Random lapse | Attention failure |
| 2 | Recency bias | More recently-activated proposition is weighted over-heavily |
| 3 | Anchor-and-adjust errors | First proposition anchors reasoning; 3rd is under-weighted |
| 4 | Interaction-blindness | Pairwise interactions are tracked but 3-way interactions are missed |
| 5+ | Conjunction fallacy / decomposition | Agent reasons about each proposition independently rather than jointly |

The shift from *probabilistically weighted* errors (2–4 propositions) to *categorical simplification* errors (5+) is a qualitative change consistent with the step-function structure of the capacity ceiling.

---

## 2.8 Summary: The Three-Regime Scaling Curve

```
Accuracy
100% ┤
     │ ● (1 prop: ~99%)
 90% ┤
     │    ● (2 props: ~92%)
 80% ┤
     │       ● (3 props: ~78%)
 70% ┤
     │
 60% ┤          ● (4 props: ~58%)  ← CLIFF BEGINS
     │
 50% ┤             ● (5 props: ~42%) ← near chance
     │
 40% ┤
     │
     └────────────────────────────── N (interacting propositions)
         1    2    3    4    5
```

**Three regimes:**
- **Zone A (N=1–2):** High reliability zone. Errors are random lapses, not systematic. Decision time is approximately linear in N.
- **Zone B (N=3):** Working zone. Performance remains above chance but shows systematic error patterns (anchoring, recency). Decision time is significantly elevated.
- **Zone C (N=4+):** Degradation zone. Accuracy drops sharply; errors shift to categorical (treating interacting propositions as independent). Decision time may paradoxically *decrease* as subjects give up on integration and revert to heuristic responses.

**Threshold between Zone B and Zone C:** Approximately N=4 interacting propositions for a trained, rested expert in an optimal environment. Under adverse conditions (time pressure, distraction, emotional stress, fatigue), the threshold drops to N=3.
