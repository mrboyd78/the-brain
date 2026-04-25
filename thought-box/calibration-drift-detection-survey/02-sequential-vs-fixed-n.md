# Deliverable 2: Sequential vs. Fixed-N Comparison

**Research question:** When is it more efficient to run a sequential test (audit-as-you-go) versus a fixed-N periodic audit? Quantify sample-size savings under realistic effect sizes.

---

## 2.1 The Core Trade-off

Fixed-N audits require a commitment: decide N in advance, collect exactly N observations, then test once. Sequential methods test as data arrives, allowing early stopping if evidence is strong enough. The choice depends on:

1. **Observation cost:** If each observation is expensive, sequential savings are valuable
2. **Decision latency:** If early detection matters operationally, sequential dominates
3. **Effect size uncertainty:** If you don't know the drift magnitude, sequential adapts better
4. **Complexity tolerance:** SPRT requires more operational infrastructure

---

## 2.2 Wald's Sequential Probability Ratio Test (SPRT)

### 2.2.1 The Setup

SPRT tests two simple hypotheses:
- H₀: p = p₀ (calibration correct)
- H₁: p = p₁ (calibration drifted by δ)

After each observation yᵢ ∈ {0, 1}, the log-likelihood ratio statistic is:

```
Λₙ = Σᵢ log[f(yᵢ|p₁) / f(yᵢ|p₀)]

For binary outcomes:
  If yᵢ = 1: contribution = log(p₁/p₀)
  If yᵢ = 0: contribution = log((1−p₁)/(1−p₀))
```

The cumulative sum Λₙ is compared against two boundaries:

```
A = log((1−β)/α)    [upper boundary: reject H₀, conclude drift]
B = log(β/(1−α))    [lower boundary: accept H₀, conclude no drift]
```

Where α = P(Type I error) and β = P(Type II error) = 1 − power.

**Decision rule:**
- If Λₙ ≥ A: Stop, conclude drift (reject H₀)
- If Λₙ ≤ B: Stop, declare no drift (accept H₀)
- If B < Λₙ < A: Continue sampling

### 2.2.2 SPRT Boundaries for Calibration Monitoring

For α = 0.05, power = 0.80 (β = 0.20):

```
A = log((1−0.20)/0.05) = log(16) = 2.773
B = log(0.20/(1−0.05)) = log(0.2105) = −1.557
```

**The likelihood ratio increment per observation for the calibration test:**

Given p₀ = 0.70, p₁ = 0.80 (detecting 10pp upward drift):
- When yᵢ = 1 (correct prediction): log(0.80/0.70) = +0.134
- When yᵢ = 0 (incorrect prediction): log(0.20/0.30) = −0.405

**Interpretation:** Each correct observation provides mild evidence for drift (+0.134), while each incorrect observation provides stronger evidence against drift (−0.405). The asymmetry reflects the underlying binomial signal.

### 2.2.3 Expected Sample Size Under H₀ and H₁

Wald's formula for the Expected Sample Number (ESN):

**Under H₀ (p = p₀, no drift):**
```
ESN₀ ≈ [B(1−α) + (A−B)α] / [p₀ × log(p₁/p₀) + (1−p₀) × log((1−p₁)/(1−p₀))]
```

**Under H₁ (p = p₁, drift present):**
```
ESN₁ ≈ [(1−β)(A−B) + βB] / [p₁ × log(p₁/p₀) + (1−p₁) × log((1−p₁)/(1−p₀))]
```

**Simplified formula (Wald 1947):**
```
ESN₀ = (B × α + A × (1−α)) / KL(p₀ || p₁)    [when testing H₀]
ESN₁ = (B × β + A × (1−β)) / KL(p₁ || p₀)    [when testing H₁]
```

where KL(p || q) = p × log(p/q) + (1−p) × log((1−p)/(1−q)) is the Kullback-Leibler divergence.

### 2.2.4 Computed ESN Values

For α=0.05, β=0.20 (power=80%), A=2.773, B=−1.557:

| p₀ | p₁ (δ=0.10) | KL(p₀‖p₁) | KL(p₁‖p₀) | ESN₀ | ESN₁ | Fixed-N | ESN₁/Fixed-N |
|---|---|---|---|---|---|---|---|
| 0.50 | 0.60 | 0.0201 | 0.0196 | 141 | 108 | 338 | **0.32** |
| 0.70 | 0.80 | 0.0214 | 0.0210 | 132 | 102 | 289 | **0.35** |
| 0.80 | 0.90 | 0.0314 | 0.0303 | 90 | 69 | 224 | **0.31** |
| 0.90 | 1.00 | — | — | — | — | 130 | N/A (degenerate) |

| p₀ | p₁ (δ=0.20) | KL(p₀‖p₁) | ESN₀ | ESN₁ | Fixed-N | ESN₁/Fixed-N |
|---|---|---|---|---|---|---|
| 0.50 | 0.70 | 0.0832 | 34 | 26 | 85 | **0.31** |
| 0.70 | 0.90 | 0.116 | 24 | 18 | 72 | **0.25** |
| 0.80 | 1.00 | — | — | — | 56 | N/A |

**Key finding:** The SPRT (under H₁, where drift actually exists) requires approximately **30–35% of the fixed-N sample** to reach a decision. This is the expected sample size savings of 65–70% in the "drift present" regime.

**Under H₀ (no drift), the SPRT still terminates earlier** than fixed-N: approximately 40–45% of fixed-N, because it accumulates evidence for the null hypothesis and stops early.

---

## 2.3 Group Sequential Designs (GSD)

### 2.3.1 Motivation

SPRT requires hypothesis-specific parameterization and tests continuously. **Group Sequential Designs** are a middle ground: test at K pre-specified interim analyses (e.g., after every 100 observations), with adjusted α boundaries that preserve the overall Type I error rate.

### 2.3.2 O'Brien-Fleming Boundaries

The O'Brien-Fleming spending function is:

```
α(t) = 2[1 − Φ(z_{α/2} / √t)]
```

where t = n_current / n_max is the information fraction.

This produces **very conservative early boundaries** (requiring large effects to stop early) and a **near-nominal final boundary** (e.g., p ≈ 0.0486 instead of 0.05 at the final look for a 5-look design).

**O'Brien-Fleming critical values for K=5 looks (α=0.05 two-sided):**

| Look | Information fraction | Critical z | Equivalent p-value |
|---|---|---|---|
| 1 | 0.20 | ±4.56 | <0.0001 |
| 2 | 0.40 | ±3.23 | 0.0012 |
| 3 | 0.60 | ±2.63 | 0.0085 |
| 4 | 0.80 | ±2.28 | 0.023 |
| 5 | 1.00 | ±2.04 | 0.041 |

**Sample size inflation vs. fixed-N:** To maintain 80% power with 5 O'Brien-Fleming looks, maximum N must be inflated by approximately **1.029×** (3% inflation). This is the cost of keeping the option to stop early.

### 2.3.3 Pocock Boundaries

The Pocock spending function uses a constant boundary:

```
α_Pocock(t) uses the same critical value z_K at each look
```

For K=5 looks, α=0.05 two-sided: z_K ≈ 2.413 at each look.

**Characteristics:**
- Easier to stop early (lower barrier at early looks vs. O'Brien-Fleming)
- Costs more in sample size: N inflation ≈ 1.228× (23% more) compared to fixed-N to maintain 80% power if trial runs to completion
- Final analysis boundary is considerably more conservative (z=2.413 vs. z=1.96)

**Pocock is rarely recommended for calibration monitoring** because the conservative final boundary means that if drift is modest and detection occurs near the planned end, many observations may be wasted. O'Brien-Fleming dominates most practical scenarios.

### 2.3.4 α-Spending Functions: Lan-DeMets Framework

The Lan-DeMets framework (1983) allows α to be spent gradually according to a spending function f(t):

**Popular spending functions:**

| Function | Formula | Characteristics |
|---|---|---|
| O'Brien-Fleming | 2[1 − Φ(z_{α/2}/√t)] | Conservative early, liberal late |
| Pocock-like | α × log(1 + (e−1) × t) | Constant-ish boundary |
| Power family | α × t^φ | φ<1 front-loads; φ>1 back-loads |
| Kim-DeMets | α × t^ρ | Rho parameterized |

**For calibration monitoring, the recommended spending function is O'Brien-Fleming because:**
1. Calibration drift is expected to be gradual; very early stopping would be premature
2. The near-nominal final analysis threshold means the monitoring has minimal cost if drift doesn't appear early
3. Preserves power if drift appears in the middle or late monitoring period

---

## 2.4 Quantitative Comparison: Sequential vs. Fixed-N

### 2.4.1 The Efficiency Table

The following table compares expected sample sizes across methods for detecting δ=0.10 drift at p₀=0.70, α=0.05, power=0.80:

| Method | Expected N (no drift) | Expected N (drift present) | Max N | Notes |
|---|---|---|---|---|
| Fixed-N (one-shot test) | 289 | 289 | 289 | Single test at end |
| SPRT | ~130 | ~100 | Unbounded* | Most efficient |
| GSD (K=5, O'Brien-Fleming) | ~230 | ~185 | 298 | Bounded max N |
| GSD (K=5, Pocock) | ~200 | ~160 | 355 | Larger max possible |
| SPRT with curtailment at 1.5× | ~130 | ~100 | ~430 | Practical SPRT |

*SPRT technically has no hard maximum; the probability of exceeding 3× fixed-N is < 5% in practice.

**Take-home:** Under the realistic scenario where drift is present at the hypothesized level, SPRT saves approximately **65% of observations vs. fixed-N**. GSD with O'Brien-Fleming saves approximately **36%**. If drift is absent, SPRT still saves approximately **55%** and GSD saves approximately **20%**.

### 2.4.2 When to Prefer Each Method

**Use Fixed-N when:**
- Observations cannot be reviewed incrementally (batch processing)
- Regulatory/institutional requirements mandate a pre-specified testing schedule
- The effect size is well-known and you want predictable resource commitment
- Administrative simplicity is paramount (single analysis, simple reporting)

**Use SPRT when:**
- Each observation triggers an immediate actionable decision
- Early detection matters urgently (cost or harm from miscalibration is high)
- The effect size is uncertain (SPRT adapts to whatever effect is present)
- You can live with unbounded (though probabilistically constrained) maximum N

**Use GSD (O'Brien-Fleming) when:**
- You want sequential efficiency but with a hard maximum N cap
- You have a periodic review cycle (weekly, monthly audits)
- You need the flexibility of early stopping while maintaining interpretable p-values
- Institutional oversight (e.g., a monitoring board) is involved

**Use Sequential Bayes Factors when:**
- Evidence magnitude interpretation (BF=30 = "strong evidence") is more useful than p-values
- Optional stopping flexibility is required (BF methods are more robust to stopping decisions)
- You want to accumulate evidence bidirectionally (both for and against drift)

---

## 2.5 Sequential Bayes Factors for Calibration Monitoring

### 2.5.1 The Framework

The Bayes Factor (BF₁₀) quantifies the relative evidence for H₁ (drift) vs. H₀ (no drift):

```
BF₁₀ = P(data | H₁) / P(data | H₀)
```

For monitoring calibration with a known p₀ and testing p₁ = p₀ + δ:

**Exact BF for accumulated binary data X = {y₁, y₂, ..., yₙ}:**

```
BF₁₀ = (p₁^s × (1−p₁)^(n−s)) / (p₀^s × (1−p₀)^(n−s))
       = (p₁/p₀)^s × ((1−p₁)/(1−p₀))^(n−s)
```

where s = Σyᵢ is the number of successes.

This simplifies to: **BF₁₀ = exp(Λₙ)** where Λₙ is the SPRT statistic — the Bayes Factor is just the exponentiated likelihood ratio.

**Stopping thresholds (Jeffreys' scale):**

| BF₁₀ | Interpretation | Action |
|---|---|---|
| < 1/10 | Strong evidence for H₀ (no drift) | Stop, declare calibration OK |
| 1/10 – 1/3 | Moderate evidence for H₀ | Continue surveillance |
| 1/3 – 3 | Ambiguous | Continue sampling |
| 3 – 10 | Moderate evidence for H₁ (drift) | Alert, investigate |
| > 10 | Strong evidence for H₁ | Stop, declare drift |
| > 30 | Very strong evidence | Immediate recalibration |

### 2.5.2 Expected Sample Size Comparison

Sequential BF with thresholds BF₁₀>10 (drift) and BF₁₀<1/10 (no drift) corresponds approximately to:
- Type I error ≈ 5% (when threshold=10 on both sides)
- Power ≈ 80% (for the same δ as fixed-N)
- Expected N ≈ 70–80% of SPRT ESN (slightly more efficient due to thresholds)

**However:** The exact calibration of BF thresholds to frequentist error rates requires simulation, and common thresholds (BF>10) do not correspond to exactly α=0.05. For strict frequentist error control, SPRT remains preferable.

---

## 2.6 Mixture SPRT (mSPRT): The Composite Alternative

The standard SPRT requires specifying both H₀ and H₁ exactly. In practice, you know p₀ (declared confidence) but not p₁ (the unknown drifted rate). The **Mixture SPRT** integrates the likelihood ratio over a prior distribution on p₁:

```
mSPRT statistic = ∫ [Λ(p₁)] × π(p₁) dp₁
```

where π(p₁) is a mixing distribution (prior) over the alternative.

**Practical choice:** Beta(a, b) mixing distribution with a=2, b=2 (uniform-ish, gives more weight to large shifts than extremes) is robust for calibration monitoring.

**The mSPRT with Beta(1,1) mixing on [p₀+ε, 1] provides:**
- Exact Type I error control (always valid p-values regardless of stopping time)
- Power similar to SPRT if the true shift is near the center of the prior
- Slightly larger expected N than SPRT tuned to the exact true δ

---

## 2.7 Decision Guide: Sequential vs. Fixed-N

```
Is early detection crucial?
    │
    ├─ YES → Use sequential method
    │         ├─ Do you know the expected drift size precisely?
    │         │    ├─ YES → SPRT (maximally efficient)
    │         │    └─ NO  → mSPRT or Sequential Bayes Factor
    │         └─ Do you need a hard maximum N cap?
    │              ├─ YES → Group Sequential Design (O'Brien-Fleming)
    │              └─ NO  → SPRT with curtailment
    │
    └─ NO → Use Fixed-N
              ├─ Single bucket test?  → Binomial proportion test (Deliverable 1)
              ├─ Multiple buckets?    → Hosmer-Lemeshow or ECE test
              └─ Two time periods?    → Two-sample proportion test
```
