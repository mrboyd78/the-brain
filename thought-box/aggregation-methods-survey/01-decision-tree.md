# Deliverable 1: Decision Tree — Selecting the Right Aggregation Method

> **Survey**: Aggregation Methods for Combining Multiple Criterion Scores  
> **Topic**: Criterion-property-driven method selection  
> **Date Compiled**: April 2026

---

## 1.1 Purpose and Scope

The choice of aggregation function is not cosmetic — it encodes a substantive claim about the structure of the problem: whether poor performance on one dimension can be offset by excellence on another, whether the scales are comparable, and whether the observations are statistically independent. Choosing wrongly changes rank orderings, distorts sensitivity, and produces composite scores that cannot be traced back to any coherent preference or probability model.

This deliverable provides a formal decision tree and supporting analysis. It draws on:
- Multi-attribute utility theory (Keeney & Raiffa, 1976)
- Reliability and extreme-value theory (Barlow & Proschan, 1975)
- Social choice theory (Arrow, 1951; May, 1952)
- Robust composite index design (OECD, 2008; Munda, 2005)

---

## 1.2 The Five Key Diagnostic Questions

Before selecting a method, the analyst must answer five diagnostic questions in order. Each answer substantially narrows the feasible method set.

---

### Q1. What is the scale type of the criterion scores?

**Ordinal (ranks, ordered categories)**  
→ Cardinal arithmetic (AM, GM, weighted sum) is *not* justified because differences between consecutive ranks are not equal  
→ Feasible methods: Borda count, Condorcet pairwise, approval voting, lexicographic ordering  
→ Proceed to Q2 only if scores are at least interval-scaled

**Cardinal interval or ratio scale (numeric scores with meaningful differences)**  
→ Full method set is open  
→ Proceed to Q2

*Diagnostic test*: Can you meaningfully say "alternative A is twice as good as B on criterion i"? If not, treat as ordinal.

---

### Q2. Are the criteria *compensable* — can excellence on one criterion genuinely offset deficiency on another?

**Non-compensable criteria** are those where:
- A zero (or floor value) on that criterion makes the overall alternative unacceptable regardless of other scores
- The criterion represents a hard constraint, safety threshold, or legal minimum
- Failure on that criterion is irreversible or catastrophic

**Fully compensable criteria** are those where:
- Trade-offs are acceptable: a candidate who is weaker on dimension i but stronger on j is not worse than one superior on i
- The decision-maker can explicitly state an indifference rate between dimensions

| Compensability Type | Appropriate Methods | Excluded Methods |
|---|---|---|
| **Non-compensable (any criterion)** | `min()`, weighted-min, lexicographic, OWA with low orness | Arithmetic mean, unweighted Borda |
| **Fully compensable** | Arithmetic mean, weighted sum, additive MAUT | `min()`, OWA with low orness |
| **Partially compensable** | Geometric mean, OWA (intermediate orness), multiplicative MAUT | Pure `min()`, pure arithmetic mean |

*Diagnostic test*: Can the decision-maker specify a trade-off rate "how many extra points on criterion j compensate for one point lost on criterion i"? If yes → compensable. If the answer is "no amount", → non-compensable for that pair.

---

### Q3. Are the failure modes *independent* across criteria?

This question is most critical when `min()` or product-form (fault-tree) aggregation is under consideration, and when the domain is safety or risk.

**Independent failures**: The event that criterion i falls below threshold is statistically unrelated to criterion j falling below threshold. Examples: physical failure of distinct, non-shared components.

**Correlated/dependent failures**: A common cause (e.g., shared subsystem, same environmental stress, same measurement instrument) drives multiple criteria simultaneously. Examples: common-cause failures in nuclear plants; a single evaluator scoring all criteria for one alternative.

| Failure Mode Structure | Implication for min() | Implication for product-form |
|---|---|---|
| Independent | `min()` correctly identifies the bottleneck | Product of probabilities is correct: P(fail) = 1 − ∏Rᵢ |
| Positively correlated | `min()` over-penalizes: the worst criterion tends to drag others down too, so the effective worst case is already captured by individual marginals | Product form *underestimates* joint failure probability (actual P(fail) > 1 − ∏Rᵢ) |
| Negatively correlated | `min()` may be too conservative: the worst criterion is compensated in expectation | Rare in practice; product form overestimates |

*Diagnostic test*: Is there a common cause that can drive multiple criteria to low values simultaneously? If yes, treat as correlated.

---

### Q4. What is the noise profile of the criterion measurements?

Measurement error is not neutral — it interacts with the aggregation method to produce systematic bias or inflated variance in the composite.

**High noise, many criteria**: Arithmetic mean benefits from noise cancellation (σ_composite ≈ σ_individual / √n for i.i.d. errors). `min()` *concentrates* noise in whichever dimension happens to score worst, even if that is a noise artifact.

**Low noise, safety-critical**: Noise concern is secondary; structural properties (compensability, independence) dominate.

**Noise concentrated in one criterion**: AM dilutes it; `min()` magnifies it if that criterion happens to also have the lowest true score; geometric mean partially penalizes it.

| Noise Scenario | Favored Method | Avoid |
|---|---|---|
| High i.i.d. noise across all criteria | Arithmetic mean | `min()` |
| Noise concentrated in suspected weakest criterion | Weighted methods with reduced weight on noisy dim | Raw `min()` |
| Low noise, independent failures | `min()` or product-form | No specific exclusion |
| Noise heterogeneous; some criteria more reliable | Weighted mean (weights ∝ reliability) | Equal-weight AM |

*Diagnostic test*: What is the estimated measurement standard deviation on each criterion as a fraction of its range? If σ/range > 0.15 on any criterion, that criterion's noise will substantially affect non-compensatory methods.

---

### Q5. Is the aggregation for ranking, certification, or probability estimation?

The purpose of the composite determines which mathematical properties are required.

**Ranking / preference ordering** (select the best alternative):  
- Ordinal consistency is required; cardinal precision is optional  
- Borda, Condorcet, or rank-based methods are formally appropriate  
- Weighted sum is acceptable if compensability is confirmed  
- Arrow's theorem (1951) applies: no ordinal method simultaneously satisfies Pareto, IIA, and non-dictatorship for >2 alternatives

**Certification / pass-fail** (determine if an alternative meets all thresholds):  
- Non-compensable structure; each criterion is a hard gate  
- Correct method: conjunctive rule (`min()` ≥ threshold, or equivalently all criteria ≥ their individual thresholds)  
- Do NOT use AM for certification: an average above threshold allows individual criteria to be below

**Probability / risk estimation** (estimate system failure probability):  
- Use reliability algebra (Barlow & Proschan, 1975): series system = product of reliabilities (= min of failure events under independence)  
- Use fault-tree / bow-tie for complex topologies  
- F-N curves aggregate scenario-level (frequency × consequence) pairs across all top events

---

## 1.3 Decision Tree (Text Form)

```
START: What scale are the criterion scores?
│
├── ORDINAL (ranks, categories)
│   └── → USE: Borda count, Condorcet pairwise, or approval voting
│           Flag: IIA violation (Borda); cycling risk (Condorcet)
│
└── CARDINAL (interval or ratio)
    │
    ├── Are any criteria NON-COMPENSABLE?
    │   │
    │   ├── YES (hard constraints / safety gates)
    │   │   │
    │   │   ├── Are failure modes INDEPENDENT?
    │   │   │   ├── YES → USE: min() or conjunctive threshold rule
    │   │   │   │           (Barlow & Proschan 1975; series system model)
    │   │   │   └── NO (correlated failures) → USE: Copula-based or 
    │   │   │           common-cause failure model; min() overstates independence
    │   │   │
    │   │   └── Is measurement noise high on the threshold criterion?
    │   │       ├── YES → USE: Weighted-min with reliability-adjusted weights;
    │   │       │           or fuzzy threshold; raw min() is noise-amplifying
    │   │       └── NO  → min() is appropriate
    │   │
    │   └── NO (all criteria are compensable)
    │       │
    │       ├── Are trade-off rates CONSTANT across the score range?
    │       │   ├── YES → USE: Weighted arithmetic mean (additive MAUT)
    │       │   │           (Keeney & Raiffa 1976, additive form)
    │       │   └── NO (diminishing returns; penalty for imbalance)
    │       │       └── USE: Geometric mean or multiplicative MAUT
    │       │               (Keeney & Raiffa 1976, multiplicative form)
    │       │
    │       └── Is the goal EPISTEMIC (aggregating diverse estimates)?
    │           └── YES → USE: Simple mean or diversity-weighted aggregation
    │                       (Page 2007 DPT; Condorcet Jury Theorem)
    │
    └── MIXED (some compensable, some not)
        └── USE: OWA operator (Yager 1988) with orness ∈ (0,1);
                or hybrid: hard floor on non-compensable criteria,
                then weighted mean on remaining criteria
```

---

## 1.4 Method Properties Reference Table

| Method | Scale Required | Compensability | Independence Assumption | Noise Behavior | Primary Reference |
|---|---|---|---|---|---|
| `min()` | Cardinal | None | Independent failure modes | Concentrates in lowest | Barlow & Proschan (1975) |
| Weighted-min | Cardinal | None | Independent | Concentrates; weight modulates | — |
| Arithmetic mean | Cardinal | Full | None required | Reduces by 1/√n | Keeney & Raiffa (1976) |
| Geometric mean | Cardinal (ratio preferred) | Partial | None required | Penalizes low values; sensitive to near-zero | Keeney & Raiffa (1976) multiplicative |
| OWA | Cardinal | Tunable (orness parameter) | None required | Depends on orness | Yager (1988) |
| Weighted sum / additive MAUT | Cardinal | Full | Attribute utility independence | Reduces by weighted 1/√n | Keeney & Raiffa (1976) |
| Borda count | Ordinal | Partial (positional) | None required | Robust (ordinal, no cardinal noise) | Arrow (1951); May (1952) |
| Condorcet methods | Ordinal | None (pairwise) | None required | Cycling risk; no noise in scores | List & Goodin (2001); Dietrich (2008) |
| Approval voting | Ordinal/binary | None | None required | Threshold sensitivity | Brams & Fishburn (1983) |
| Diversity-weighted mean | Cardinal | Full | Errors must be diverse | Reduces if errors uncorrelated | Page (2007, 2017) |
| Fault-tree / series product | Probability | None | **Must** be independent | Propagates as product | Barlow & Proschan (1975) |

---

## 1.5 Critical Assumption Violations and Their Consequences

### 1.5.1 Using AM When Criteria Are Non-Compensable
**What goes wrong**: An alternative that scores 0 on a safety-critical criterion can still receive a passing composite score if it scores high on all other criteria. This is the fundamental error in applying AM to certification problems.

*Example*: A nuclear containment system scored: structural integrity = 0.95, operator training = 0.90, containment strength = 0.00 (failed seismic test). AM = 0.617 — appears to pass. `min()` = 0.00 — correctly flags failure.

### 1.5.2 Using `min()` When Criteria Are Correlated
**What goes wrong**: Correlated criteria share a common cause. If criterion A and criterion B are driven by the same underlying latent factor, then a low score on A already implies a low score on B. Taking `min()` of two correlated criteria does *not* add information beyond the lower one alone — it just re-penalizes the same underlying weakness twice, producing an artificially harsh composite.

*Example*: Two sub-scores derived from the same test session (fatigue affects both) are correlated ~0.80. Taking min() treats them as independent bottlenecks; they are actually the same bottleneck measured twice.

### 1.5.3 Using Ordinal Aggregation (Borda) When Preference Intensity Matters
**What goes wrong**: Borda counts weight each rank position equally regardless of how far apart the underlying scores are. An alternative that is barely better than second-place on all criteria beats one that is vastly superior on most criteria but slightly inferior on one — a violation of proportionality.

*Example*: Alternative X scores [10, 10, 10, 10, 1] and alternative Y scores [6, 6, 6, 6, 6]. Borda with five alternatives may rank Y above X if Y consistently edges X by one rank position across criteria.

### 1.5.4 Aggregating Across Heterogeneous Sub-Populations (Simpson's Paradox)
**What goes wrong**: When sub-groups differ in size and average criterion scores, the aggregate reverses the within-group ordering. The formal conditions for Simpson's Paradox reversal are: (a) heterogeneous sub-populations, (b) different within-group baselines, (c) unequal sub-group sizes.

*Resolution*: Stratify before aggregating; use weighted aggregation with sub-group size as the weight only when the sub-groups are genuinely comparable.

---

## 1.6 Special Case: Risk and Safety Aggregation

When criteria are failure probabilities or reliability values, the aggregation is governed by structural reliability theory (Barlow & Proschan, 1975):

**Series system** (all components must work): 
```
R_system = ∏ Rᵢ   (under independence)
```
This is equivalent to a *minimum on the reliability scale* and a *sum on the log-failure-rate scale*.

**Parallel system** (any one component suffices):
```
R_system = 1 − ∏(1 − Rᵢ)
```
This is a *maximum* operator on the reliability domain.

**Fault-tree and bow-tie models** compute system failure probability bottom-up from component-level failure rates through AND gates (series logic) and OR gates (parallel logic). F-N curves then aggregate the frequency-consequence pairs across all top events to yield a societal risk profile. The F-N curve is an integral over the consequence distribution, not a simple linear sum.

---

## 1.7 Key Citations

- Arrow, K. J. (1951). *Social Choice and Individual Values.* Wiley.
- Barlow, R. E., & Proschan, F. (1975). *Statistical Theory of Reliability and Life Testing: Probability Models.* Holt, Rinehart and Winston.
- Brams, S. J., & Fishburn, P. C. (1983). *Approval Voting.* Birkhäuser.
- Dietrich, F. (2008). The premises of Condorcet's jury theorem are not simultaneously justified. *Episteme*, 5(1), 56–73.
- Keeney, R. L., & Raiffa, H. (1976). *Decisions with Multiple Objectives: Preferences and Value Trade-offs.* Wiley.
- List, C., & Goodin, R. E. (2001). Epistemic democracy: Generalizing the Condorcet jury theorem. *Journal of Political Philosophy*, 9(3), 277–306.
- May, K. O. (1952). A set of independent necessary and sufficient conditions for simple majority decision. *Econometrica*, 20(4), 680–684.
- Munda, G. (2005). Multiple criteria decision analysis and sustainable development. In J. Figueira et al. (Eds.), *Multiple Criteria Decision Analysis: State of the Art Surveys.* Springer.
- OECD/JRC. (2008). *Handbook on Constructing Composite Indicators.* OECD Publishing.
- Page, S. E. (2007). *The Difference.* Princeton University Press.
- Page, S. E. (2017). *The Diversity Bonus.* Princeton University Press.
- Yager, R. R. (1988). On ordered weighted averaging aggregation operators in multicriteria decisionmaking. *IEEE Transactions on Systems, Man, and Cybernetics*, 18(1), 183–190.
