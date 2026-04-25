# Aggregation Methods Survey — Index

> **Topic**: Combining Multiple Criterion Scores into a Single Composite  
> **Focus**: When each method is appropriate and when it fails  
> **Date Compiled**: April 2026

---

## Deliverables

| # | File | Topic |
|---|---|---|
| 1 | [01-decision-tree.md](./01-decision-tree.md) | Decision tree: which aggregation method given criterion properties |
| 2 | [02-min-aggregation.md](./02-min-aggregation.md) | `min()` specifically: correct and incorrect conditions |
| 3 | [03-worked-comparison.md](./03-worked-comparison.md) | Worked 6-criterion comparison: `min`, weighted-min, AM, GM, Borda |
| 4 | [04-noise-sensitivity.md](./04-noise-sensitivity.md) | Noise propagation under each aggregation method |
| 5 | [05-use-case-recommendations.md](./05-use-case-recommendations.md) | Recommended aggregation for three generic use cases |

---

## Method Taxonomy

```
Aggregation Methods
│
├── Non-compensatory
│   ├── min() / weakest-link         [Barlow & Proschan 1975]
│   ├── Weighted-min
│   └── Lexicographic ordering
│
├── Partially compensatory
│   ├── Geometric mean               [Keeney & Raiffa 1976 multiplicative MAUT]
│   ├── OWA (Ordered Weighted Avg)   [Yager 1988]
│   └── TOPSIS / outranking methods
│
├── Fully compensatory
│   ├── Arithmetic mean / weighted sum [Keeney & Raiffa additive MAUT]
│   └── Multiplicative utility (global) [Keeney & Raiffa Ch.6]
│
└── Voting-based / ordinal
    ├── Majority rule                [May 1952; Arrow 1951]
    ├── Borda count                  [de Borda 1784]
    ├── Approval voting              [Brams & Fishburn 1983]
    ├── Condorcet methods            [List & Goodin 2001; Dietrich 2008]
    └── Diversity-weighted avg       [Page 2007, 2017]
```

---

## Core Theoretical Anchors

| Source | Contribution |
|---|---|
| Barlow & Proschan (1975) | Series/parallel system reliability; formal weakest-link model |
| Keeney & Raiffa (1976) | Additive and multiplicative MAUT; independence axioms |
| Arrow (1951) | Impossibility theorem for ordinal aggregation |
| May (1952) | Axiomatic foundations of majority rule |
| List & Goodin (2001) | Generalized Condorcet Jury Theorem |
| Dietrich (2008) | CJT premise critique under dependence |
| Page (2007, 2017) | Diversity Prediction Theorem; cognitive diversity aggregation |
| Yager (1988) | Ordered Weighted Averaging (OWA) operators |

---

## Key Cross-Cutting Themes

- **Compensability** is the single most important choice axis: is a zero in one criterion catastrophic or recoverable?
- **Independence** of failure modes determines whether `min()` or product-form risk aggregation is valid
- **Noise concentration**: `min()` concentrates all noise in the worst-measured dimension; AM distributes it by 1/√n
- **Simpson's Paradox**: aggregation across heterogeneous sub-populations can reverse the true ordering even when within-group orderings are consistent
- **Arrow's theorem**: no ordinal aggregation rule simultaneously satisfies all four fairness axioms — designers must choose which to sacrifice
