# Deliverable 3: Worked Comparison — 6-Criterion Problem Across Five Aggregation Methods

> **Survey**: Aggregation Methods for Combining Multiple Criterion Scores  
> **Topic**: min, weighted-min, arithmetic mean, geometric mean, Borda — rank-order comparisons  
> **Date Compiled**: April 2026

---

## 3.1 Problem Setup

### 3.1.1 Scenario Description

We evaluate **six alternatives** (A–F) on **six criteria** (C1–C6). The scenario is a hybrid decision problem: a technology procurement evaluation where:

- **C1**: Security compliance (non-compensable — a legal requirement)
- **C2**: Performance / throughput (compensable — trade-offs acceptable with C3)
- **C3**: Ease of integration (compensable with C2)
- **C4**: Vendor reliability / SLA quality (compensable)
- **C5**: Cost-effectiveness (compensable)
- **C6**: Innovation / future-proofing (compensable, lower weight)

**Score scale**: All scores are on [0, 1], expressed as unit intervals (1 = best possible, 0 = worst). Scores have been normalized to the same scale — so raw cardinal arithmetic is valid.

### 3.1.2 Raw Score Matrix

| Alternative | C1 (Security) | C2 (Perf.) | C3 (Integr.) | C4 (Vendor) | C5 (Cost) | C6 (Innov.) |
|---|---|---|---|---|---|---|
| **A** | 0.95 | 0.75 | 0.70 | 0.80 | 0.60 | 0.55 |
| **B** | 0.40 | 0.92 | 0.88 | 0.95 | 0.90 | 0.85 |
| **C** | 0.85 | 0.70 | 0.65 | 0.72 | 0.75 | 0.78 |
| **D** | 0.90 | 0.55 | 0.58 | 0.60 | 0.55 | 0.50 |
| **E** | 0.88 | 0.80 | 0.82 | 0.78 | 0.82 | 0.76 |
| **F** | 0.78 | 0.60 | 0.72 | 0.68 | 0.70 | 0.65 |

### 3.1.3 Weights for Weighted Methods

Weights reflect the importance ordering (C1 security is most critical; C6 least):

| Criterion | Weight (wᵢ) | Rationale |
|---|---|---|
| C1 (Security) | 0.30 | Non-compensable; legal requirement |
| C2 (Performance) | 0.20 | Primary business driver |
| C3 (Integration) | 0.15 | Implementation cost driver |
| C4 (Vendor) | 0.15 | Long-term relationship quality |
| C5 (Cost) | 0.12 | Budget constraint |
| C6 (Innovation) | 0.08 | Strategic option value |
| **Sum** | **1.00** | |

---

## 3.2 Method 1: `min()` — Unweighted Minimum

**Formula**: composite_min(x) = min{C1, C2, C3, C4, C5, C6}

**Computation**:

| Alternative | C1 | C2 | C3 | C4 | C5 | C6 | **min()** | **Rank** |
|---|---|---|---|---|---|---|---|---|
| A | 0.95 | 0.75 | 0.70 | 0.80 | 0.60 | 0.55 | **0.55** | 4 |
| B | 0.40 | 0.92 | 0.88 | 0.95 | 0.90 | 0.85 | **0.40** | 6 |
| C | 0.85 | 0.70 | 0.65 | 0.72 | 0.75 | 0.78 | **0.65** | 2 |
| D | 0.90 | 0.55 | 0.58 | 0.60 | 0.55 | 0.50 | **0.50** | 5 |
| E | 0.88 | 0.80 | 0.82 | 0.78 | 0.82 | 0.76 | **0.76** | 1 |
| F | 0.78 | 0.60 | 0.72 | 0.68 | 0.70 | 0.65 | **0.65** | 2 (tie) |

**Ranking**: E > C = F > A > D > B

**Observation**: Alternative B, which excels on five of six criteria, ranks last because of its catastrophically low security score (0.40). If C1 is genuinely non-compensable, this is correct behavior. Alternative E wins because it has the highest floor across all criteria.

---

## 3.3 Method 2: Weighted-Min

**Formula**: composite_wmin(x) = min{wᵢ · sᵢ}

This weights each criterion score before taking the minimum, so higher-weight criteria effectively raise the bar for their contribution.

Wait — there are several formulations of weighted-min. We use the most common: identify the minimum across *weight-adjusted deficit scores*, i.e., apply weights inversely (the criterion that has the most weight must be the least deficient):

**Formula used**: composite_wmin(x) = min{sᵢ · (wᵢ / max_w)} where max_w = 0.30

Equivalently: normalize each score by (weight / max_weight) = (wᵢ / 0.30):

| Criterion | Weight | Normalization Factor (wᵢ/0.30) |
|---|---|---|
| C1 | 0.30 | 1.00 |
| C2 | 0.20 | 0.667 |
| C3 | 0.15 | 0.500 |
| C4 | 0.15 | 0.500 |
| C5 | 0.12 | 0.400 |
| C6 | 0.08 | 0.267 |

**Weight-adjusted scores** (sᵢ × normalization factor):

| Alt | C1×1.0 | C2×0.667 | C3×0.500 | C4×0.500 | C5×0.400 | C6×0.267 | **wmin** | **Rank** |
|---|---|---|---|---|---|---|---|---|
| A | 0.950 | 0.500 | 0.350 | 0.400 | 0.240 | 0.147 | **0.147** | 5 |
| B | 0.400 | 0.614 | 0.440 | 0.475 | 0.360 | 0.227 | **0.227** | 3 |
| C | 0.850 | 0.467 | 0.325 | 0.360 | 0.300 | 0.208 | **0.208** | 4 |
| D | 0.900 | 0.367 | 0.290 | 0.300 | 0.220 | 0.134 | **0.134** | 6 |
| E | 0.880 | 0.534 | 0.410 | 0.390 | 0.328 | 0.203 | **0.203** | 5 (tie ~) |
| F | 0.780 | 0.400 | 0.360 | 0.340 | 0.280 | 0.174 | **0.174** | 4 |

**Note**: In this formulation, lower-weight criteria are allowed to be lower in absolute terms because they are less critical. The minimum of weight-adjusted scores captures the binding deficiency accounting for importance.

Recalculated properly (min of adjusted scores):

| Alt | wmin value | Rank |
|---|---|---|
| E | 0.203 | **1** |
| B | 0.227 → but bottleneck is C1: 0.400 | **2** |
| F | 0.174 | **3** |
| C | 0.208 | **4** |
| A | 0.147 | **5** |
| D | 0.134 | **6** |

**Observation vs. unweighted min**: B improves substantially from rank 6 to rank 2, because its catastrophic security score (0.40) is the binding constraint but the weight adjustment means low-weight criteria are automatically discounted. The low-innovation score (0.267 adjusted) was the unweighted min bottleneck for alternatives with otherwise good scores. D falls to last because its relatively weak performance on the *high-weight* criteria (C2 × 0.667 = 0.367 is its bottleneck after adjustment).

---

## 3.4 Method 3: Weighted Arithmetic Mean

**Formula**: composite_AM(x) = ∑ wᵢ · sᵢ

| Alt | w₁·C1 | w₂·C2 | w₃·C3 | w₄·C4 | w₅·C5 | w₆·C6 | **AM** | **Rank** |
|---|---|---|---|---|---|---|---|---|
| A | 0.285 | 0.150 | 0.105 | 0.120 | 0.072 | 0.044 | **0.776** | 3 |
| B | 0.120 | 0.184 | 0.132 | 0.143 | 0.108 | 0.068 | **0.755** | 4 |
| C | 0.255 | 0.140 | 0.098 | 0.108 | 0.090 | 0.062 | **0.753** | 5 |
| D | 0.270 | 0.110 | 0.087 | 0.090 | 0.066 | 0.040 | **0.663** | 6 |
| E | 0.264 | 0.160 | 0.123 | 0.117 | 0.098 | 0.061 | **0.823** | 1 |
| F | 0.234 | 0.120 | 0.108 | 0.102 | 0.084 | 0.052 | **0.700** | 5 (tied ~) |

**Detailed computation for A**: (0.30×0.95) + (0.20×0.75) + (0.15×0.70) + (0.15×0.80) + (0.12×0.60) + (0.08×0.55)
= 0.285 + 0.150 + 0.105 + 0.120 + 0.072 + 0.044 = **0.776**

**Ranking**: E > A > B > C > F > D

**Critical observation**: Alternative B, which scored 0.40 on C1 (security), receives a weighted mean of 0.755 — placing it *third-best*. This is the fundamental flaw of AM for non-compensable criteria: B's excellent scores on C2–C6 nearly fully compensate for its security failure. If C1 is a legal requirement, B should not be in the top three.

---

## 3.5 Method 4: Weighted Geometric Mean

**Formula**: composite_GM(x) = ∏ (sᵢ^wᵢ)

| Alt | C1^0.30 | C2^0.20 | C3^0.15 | C4^0.15 | C5^0.12 | C6^0.08 | **GM** | **Rank** |
|---|---|---|---|---|---|---|---|---|
| A | 0.9832 | 0.9449 | 0.9466 | 0.9666 | 0.9316 | 0.9549 | **0.770** | 3 |
| B | 0.8708 | 0.9831 | 0.9820 | 0.9929 | 0.9884 | 0.9860 | **0.700** | 4 |
| C | 0.9537 | 0.9376 | 0.9386 | 0.9568 | 0.9666 | 0.9795 | **0.749** | 5 |
| D | 0.9685 | 0.8832 | 0.9083 | 0.9316 | 0.9253 | 0.9253 | **0.655** | 6 |
| E | 0.9629 | 0.9564 | 0.9717 | 0.9641 | 0.9786 | 0.9779 | **0.815** | 1 |
| F | 0.9253 | 0.8974 | 0.9466 | 0.9453 | 0.9575 | 0.9601 | **0.699** | 5 (≈) |

**Detailed computation for A**:
- C1^0.30 = 0.95^0.30 = e^(0.30 × ln 0.95) = e^(0.30 × −0.0513) = e^(−0.0154) = 0.9847
- C2^0.20 = 0.75^0.20 = e^(0.20 × −0.2877) = e^(−0.0575) = 0.9441
- C3^0.15 = 0.70^0.15 = e^(0.15 × −0.3567) = e^(−0.0535) = 0.9479
- C4^0.15 = 0.80^0.15 = e^(0.15 × −0.2231) = e^(−0.0335) = 0.9671
- C5^0.12 = 0.60^0.12 = e^(0.12 × −0.5108) = e^(−0.0613) = 0.9405
- C6^0.08 = 0.55^0.08 = e^(0.08 × −0.5978) = e^(−0.0478) = 0.9533
- Product = 0.9847 × 0.9441 × 0.9479 × 0.9671 × 0.9405 × 0.9533 ≈ **0.770**

**Ranking**: E > A > B > C ≈ F > D

**Observation**: GM partially penalizes B's low security score — note B drops to 0.700 vs. AM's 0.755 for B. The GM is more sensitive to low values because it uses logarithmic averaging. However, unlike `min()`, GM still allows B to rank 4th rather than last; the penalty is partial, not absolute.

---

## 3.6 Method 5: Borda Count

**Formula**: For each criterion, rank all alternatives from best (rank 1) to worst (rank 6). Assign Borda points = (n − rank) = (6 − rank). Sum Borda points across criteria.

**Step 1: Rank alternatives within each criterion** (1 = best):

| Alt | C1 rank | C2 rank | C3 rank | C4 rank | C5 rank | C6 rank |
|---|---|---|---|---|---|---|
| A | 1 | 3 | 4 | 2 | 6 | 6 |
| B | 6 | 1 | 1 | 1 | 1 | 1 |
| C | 3 | 4 | 5 | 4 | 3 | 3 |
| D | 2 | 6 | 6 | 6 | 5 | 5 |
| E | 4 | 2 | 2 | 3 | 2 | 4 |
| F | 5 | 5 | 3 | 5 | 4 | 2 |

**Step 2: Borda points** = 6 − rank (5 = best, 0 = worst):

| Alt | C1 pts | C2 pts | C3 pts | C4 pts | C5 pts | C6 pts | **Total** | **Rank** |
|---|---|---|---|---|---|---|---|---|
| A | 5 | 3 | 2 | 4 | 0 | 0 | **14** | 4 |
| B | 0 | 5 | 5 | 5 | 5 | 5 | **25** | 1 |
| C | 3 | 2 | 1 | 2 | 3 | 3 | **14** | 4 (tie) |
| D | 4 | 0 | 0 | 0 | 1 | 1 | **6** | 6 |
| E | 2 | 4 | 4 | 3 | 4 | 2 | **19** | 2 |
| F | 1 | 1 | 3 | 1 | 2 | 4 | **12** | 3 → |

Correcting: F = 12, A = 14, C = 14 → ranking:

**Ranking**: B > E > A = C > F > D

**Shocking observation**: Alternative B, which has the lowest security score, **wins** the Borda count with 25 points! The Borda method treats each criterion rank equally — B's 5 first-place finishes on criteria C2–C6 completely dominate A's strongest criterion (C1). 

This is the fundamental property of Borda: it is a fully compensatory ordinal method. It has no mechanism for non-compensability, and since it ignores cardinal magnitudes, the fact that B's security score is 0.40 (a genuine failure) is treated identically to being last by one ordinal position.

---

## 3.7 Full Rank Comparison Table

| Alternative | min() | Weighted-min | Arithmetic Mean | Geometric Mean | Borda |
|---|---|---|---|---|---|
| **A** | 4 | 5 | **3** | **3** | 3 |
| **B** | **6** | **2** | 4 | 4 | **1** |
| **C** | 2 | 4 | 5 | 5 | 3 |
| **D** | 5 | 6 | 6 | 6 | 6 |
| **E** | **1** | **1** | **1** | **1** | **2** |
| **F** | 2 | 3 | 5 | 4 | 5 |

### Key Rank-Order Reversals

| Alternative | Movement | Cause |
|---|---|---|
| **B** | 6th (min) → 1st (Borda) | B dominates on 5 criteria; min/wmin picks up security failure; AM/GM partially penalize; Borda ignores magnitudes |
| **C** | 2nd (min) → 5th (AM) | C has a good floor but mediocre everywhere except the lowest criterion; AM exposes mediocrity |
| **A** | 4th (min) → 3rd (AM/GM) | A has a very high security score pulling up AM/GM; min() is dragged down by innovation score |
| **E** | 1st (min, wmin, AM, GM) → 2nd (Borda) | E is consistently strong but not first on any single criterion; Borda rewards domination |

---

## 3.8 Decision Implications by Use Case

### Use Case 1: Security certification (non-compensable C1)

**Correct method**: `min()` or conjunctive threshold (eliminate B first; rank remainder by secondary method)

**What each method does**:
- `min()` → correctly disqualifies B (last)
- `wmin` → partially flags B (second from last after D)
- AM → incorrectly allows B to rank 4th; B could pass if composite threshold is 0.70
- GM → partially penalizes B (4th); B would fail only if composite threshold > 0.70
- Borda → B wins; completely inappropriate for this use case

### Use Case 2: Best balanced vendor (all criteria compensable)

**Correct method**: Weighted arithmetic mean or geometric mean

**Winner**: E under both AM (0.823) and GM (0.815) — consistent with its strong balanced performance.

### Use Case 3: Ordinal preference ranking only

**Correct method**: Borda count (if cardinal differences are unreliable or not meaningful)

**Result**: B > E > A = C > F > D. Note: B's ranking would likely change with a domain restriction rule or explicit veto on security failures.

---

## 3.9 Commentary on the Significance of Rank Reversal

The rank reversals observed above are not aberrations — they are the *natural* consequence of different methods encoding different structural assumptions. The analyst's responsibility is to choose the method whose assumptions match the problem structure:

1. **E is always top-2**: Consistent strong performance across all methods recommends E as the safest choice when method uncertainty exists.
2. **B's position is entirely method-dependent**: B is either best (Borda) or worst (min). This extreme sensitivity signals that B has a critical structural weakness (security) that makes it method-choice-sensitive.
3. **D is consistently worst**: D's low performance on multiple compensable criteria means it ranks last or near-last under all methods.
4. **C and F are mid-table under all methods**: Genuinely mediocre alternatives; no method lifts them to the top.

---

## 3.10 Key Citations

- Arrow, K. J. (1951). *Social Choice and Individual Values.* Wiley.
- Barlow, R. E., & Proschan, F. (1975). *Statistical Theory of Reliability and Life Testing.* Holt, Rinehart & Winston.
- Keeney, R. L., & Raiffa, H. (1976). *Decisions with Multiple Objectives.* Wiley.
- OECD/JRC. (2008). *Handbook on Constructing Composite Indicators.* OECD Publishing.
- Saltelli, A., Tarantola, S., Campolongo, F., & Ratto, M. (2004). *Sensitivity Analysis in Practice.* Wiley.
- Wakker, P. P. (2010). *Prospect Theory: For Risk and Ambiguity.* Cambridge University Press.
- Young, H. P. (1995). Optimal voting rules. *Journal of Economic Perspectives*, 9(1), 51–64.
