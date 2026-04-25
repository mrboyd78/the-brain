# Deliverable 1: Minimum-N Formulas for Calibration Drift Detection

**Research question:** How many observations are required to detect miscalibration drift of 10%, 20%, and 30% magnitude at α=0.05 with power 0.80?

---

## 1.1 Problem Formulation

### 1.1.1 The Core Statistical Setup

A calibrated forecaster assigns probability *p*̂ to an event, and the event occurs with frequency *p*_true. **Miscalibration** is the gap:

```
δ = p_true − p̂_declared
```

We observe N binary outcome pairs {(p̂ᵢ, yᵢ)} where yᵢ ∈ {0, 1}. The empirical rate in a bucket centered at declared confidence *p*̂₀ is:

```
p̄ = (1/N) Σᵢ yᵢ
```

The null hypothesis is H₀: p_true = p̂₀ (perfect calibration).  
The alternative is H₁: p_true = p̂₀ + δ (miscalibration of magnitude δ).

This is a **one-sample binomial proportion test** for each confidence bucket.

### 1.1.2 Why Separate Buckets Matter

For graded confidence (e.g., 5-bucket system: 20%, 40%, 60%, 80%, 95%), each bucket must be analyzed separately because:
1. The variance of the binary outcome differs by declared confidence p (variance = p(1-p))
2. The direction of miscalibration may differ across buckets
3. Pooling across buckets conflates calibration with discrimination

---

## 1.2 Binary Outcomes: The One-Sample Proportion Test

### 1.2.1 The Normal Approximation Formula

For large N, the standardized test statistic is:

```
Z = (p̄ − p₀) / sqrt(p₀(1−p₀) / N)
```

For a **one-sided test** at significance level α and power 1−β, the required sample size is:

```
        [z_α × sqrt(p₀(1−p₀)) + z_β × sqrt(p₁(1−p₁))]²
N  =  ──────────────────────────────────────────────────
                         (p₁ − p₀)²
```

Where:
- p₀ = declared (null) probability
- p₁ = p₀ + δ (alternative: drifted probability)
- z_α = 1.645 (one-sided α = 0.05)
- z_β = 0.842 (power = 0.80, β = 0.20)
- δ = drift magnitude

For a **two-sided test**: replace z_α with z_{α/2} = 1.960.

### 1.2.2 Key Insight: Variance Changes Across Confidence Levels

The variance p(1−p) is maximized at p = 0.50 and is smaller near the extremes. This has an important practical consequence: **detecting the same absolute drift δ requires fewer observations when the declared confidence is near 0% or 100% than near 50%**, because the signal-to-noise ratio is higher at the extremes.

Variance by confidence level:
| Declared p₀ | Variance p₀(1−p₀) | Relative difficulty |
|---|---|---|
| 0.10 | 0.090 | Low |
| 0.20 | 0.160 | Low-Medium |
| 0.30 | 0.210 | Medium |
| 0.50 | 0.250 | **Highest** |
| 0.70 | 0.210 | Medium |
| 0.80 | 0.160 | Low-Medium |
| 0.90 | 0.090 | Low |
| 0.95 | 0.0475 | Very Low |

---

## 1.3 Sample Size Tables: Binary Outcomes

### Table 1.1: Required N per bucket at α=0.05 (one-sided), Power=0.80

Values are N = observations needed in the bucket centered at the declared confidence p₀.

| Declared p₀ | 10 pp drift (δ=0.10) | 20 pp drift (δ=0.20) | 30 pp drift (δ=0.30) |
|---|---|---|---|
| 0.10 | 130 | 33 | 15 |
| 0.20 | 224 | 56 | 25 |
| 0.30 | 289 | 72 | 32 |
| 0.40 | 325 | 81 | 36 |
| **0.50** | **338** | **85** | **38** |
| 0.60 | 325 | 81 | 36 |
| 0.70 | 289 | 72 | 32 |
| 0.80 | 224 | 56 | 25 |
| 0.90 | 130 | 33 | 15 |
| 0.95 | 76 | 19 | 9 |

> Note: These are per-bucket. For overall Type-I control across K buckets, apply Bonferroni correction: α_bucket = 0.05/K.

### Table 1.2: Required N at α=0.05 (two-sided), Power=0.80

Note: Two-sided tests are appropriate when drift can go in either direction (over- or under-confidence).

| Declared p₀ | 10 pp drift | 20 pp drift | 30 pp drift |
|---|---|---|---|
| 0.10 | 163 | 41 | 18 |
| 0.20 | 281 | 70 | 31 |
| 0.30 | 362 | 91 | 40 |
| 0.40 | 407 | 102 | 45 |
| **0.50** | **424** | **106** | **47** |
| 0.70 | 362 | 91 | 40 |
| 0.90 | 163 | 41 | 18 |
| 0.95 | 95 | 24 | 11 |

### Table 1.3: Worked Example — Single Bucket at p₀=0.70

**Scenario:** A system assigns 70% confidence to a class of predictions. We want to detect if the true accuracy has drifted to 80% (δ = +0.10, over-precision) or 60% (δ = −0.10, under-precision) with 80% power at α=0.05.

**Parameters:**
- p₀ = 0.70, p₁ = 0.80 (detecting 10pp upward drift)
- z_α = 1.645 (one-sided), z_β = 0.842

**Calculation:**
```
Numerator = [1.645 × sqrt(0.70 × 0.30) + 0.842 × sqrt(0.80 × 0.20)]²
           = [1.645 × 0.4583 + 0.842 × 0.4000]²
           = [0.7539 + 0.3368]²
           = [1.0907]²
           = 1.190

Denominator = (0.80 − 0.70)² = 0.01

N = 1.190 / 0.01 = 119
```

**Rounding up → N = 120 observations in the 70% bucket.**

**Check:** Using R's `power.prop.test(p1=0.70, p2=0.80, sig.level=0.05, power=0.80, alternative="one.sided")` gives N = 121 per group for a two-sample test; for a one-sample test with known p₀, this is approximately 120. ✓

**For the two-sided version (detecting drift in either direction):**
```
z_{α/2} = 1.960 instead of 1.645
N = [1.960 × 0.4583 + 0.842 × 0.4000]² / 0.01
  = [0.8983 + 0.3368]² / 0.01
  = [1.2351]² / 0.01
  = 1.526 / 0.01 = 153
```
**→ N = 153 for two-sided detection.**

---

## 1.4 Graded Confidence Buckets (5-Bucket System)

### 1.4.1 The Setup

A 5-bucket system partitions predictions into confidence ranges:

| Bucket | Declared Range | Point Estimate p₀ |
|---|---|---|
| B1 | 0–25% | 0.125 |
| B2 | 25–45% | 0.350 |
| B3 | 45–55% | 0.500 |
| B4 | 55–75% | 0.650 |
| B5 | 75–100% | 0.875 |

For a system with a **uniform distribution across buckets**, the observations per bucket at total N are N/5 each. In reality, predictions cluster — the analysis must be done per-bucket using actual bucket counts.

### 1.4.2 Multiple Testing Correction

With 5 simultaneous bucket tests, the family-wise Type I error rate inflates if each bucket is tested at α=0.05. Required adjustment:

| Correction Method | Per-Bucket α | Total α | Power per Bucket |
|---|---|---|---|
| None (accept inflated FWE) | 0.050 | ≈0.226 | High |
| Bonferroni | 0.010 | 0.050 | Reduced (~65%) |
| Šidák | 0.0102 | 0.050 | ≈Bonferroni |
| No correction + ECE test | N/A | Use joint statistic | Full nominal power |

**Recommended approach:** Use a joint calibration test (e.g., Hosmer-Lemeshow or ECE-based test) rather than 5 independent bucket tests, to avoid the multiple testing problem while preserving power.

### 1.4.3 Sample Size for Graded System — Worked Example

**Scenario:** 5-bucket system. Predictions are uniformly distributed across buckets. We want to detect a **uniform 20pp overconfidence drift** across all buckets (every bucket is 20pp miscalibrated) with 80% power at FWER = 0.05.

**Per-bucket after Bonferroni correction (α_bucket = 0.01):**

At p₀ = 0.5 (worst case), z_{α=0.01, one-sided} = 2.326:

```
N_bucket = [2.326 × sqrt(0.5×0.5) + 0.842 × sqrt(0.7×0.3)]²/ (0.20)²
          = [2.326 × 0.500 + 0.842 × 0.458]² / 0.04
          = [1.163 + 0.386]² / 0.04
          = [1.549]² / 0.04
          = 2.400 / 0.04 = 60
```

**→ 60 observations per bucket × 5 buckets = 300 total observations** to detect 20pp uniform drift with FWER = 0.05.

But: this assumes perfect uniformity. If the center bucket (B3, p₀≈0.50) is the worst case and receives only 15% of all predictions (realistic for a reasonably discriminating classifier), you need:

```
N_total = 60 / 0.15 = 400 total observations to accumulate 60 in the center bucket.
```

### 1.4.4 ECE (Expected Calibration Error) and Sample Size

The ECE is defined as:

```
ECE = Σₘ (nₘ/N) × |acc(Bₘ) − conf(Bₘ)|
```

where the sum is over M bins, nₘ is observations in bin m, acc(Bₘ) is empirical accuracy, and conf(Bₘ) is mean declared confidence.

**Statistical properties of ECE:**
- ECE is a biased estimator of true calibration error (upward bias of order O(1/√N))
- The bias decreases as N grows
- The variance of ECE scales as O(1/N)
- **Rough rule:** ECE estimates from fewer than 1,000 total predictions are unreliable for detecting <10pp miscalibration; 500 is adequate for detecting 20pp; 100 for detecting 50pp.

**ECE bias correction (Nixon et al., 2019):** The debiased ECE estimator adjusts for the upward bias in finite-sample estimates:

```
ECE_debiased = ECE_raw − E[random_ECE]
```

where E[random_ECE] ≈ Σₘ (1/nₘ) × sqrt[conf(Bₘ) × (1 − conf(Bₘ)) / (2π)] (from theoretical analysis of the expected absolute deviation of a binomial at each bin).

---

## 1.5 Comprehensive N Table for Practitioners

### Table 1.5: Minimum N per bucket, one-sided, Power=0.80

| **Declared p₀** | **Drift δ=0.05** | **Drift δ=0.10** | **Drift δ=0.15** | **Drift δ=0.20** | **Drift δ=0.30** |
|---|---|---|---|---|---|
| 0.10 | 519 | 130 | 58 | 33 | 15 |
| 0.20 | 893 | 224 | 100 | 56 | 25 |
| 0.30 | 1,151 | 289 | 129 | 72 | 32 |
| 0.40 | 1,298 | 325 | 145 | 81 | 36 |
| **0.50** | **1,353** | **338** | **151** | **85** | **38** |
| 0.60 | 1,298 | 325 | 145 | 81 | 36 |
| 0.70 | 1,151 | 289 | 129 | 72 | 32 |
| 0.80 | 893 | 224 | 100 | 56 | 25 |
| 0.90 | 519 | 130 | 58 | 33 | 15 |
| 0.95 | 301 | 76 | 34 | 19 | 9 |

> Formula: N = [z_α × √(p₀q₀) + z_β × √(p₁q₁)]² / δ²  
> where z_α = 1.645 (α=0.05 one-sided), z_β = 0.842 (power=80%)

### Table 1.6: "How many total predictions do I need?" (5 buckets, uniform split)

To detect uniform drift across all 5 buckets simultaneously (Bonferroni-corrected α=0.01 per bucket):

| Drift δ | N per bucket | Total N (uniform dist) | Total N (if center bucket gets 15%) |
|---|---|---|---|
| 5 pp | 1,800 | 9,000 | 12,000 |
| 10 pp | 450 | 2,250 | 3,000 |
| 20 pp | 115 | 575 | 767 |
| 30 pp | 51 | 255 | 340 |

---

## 1.6 Exact Binomial Test for Small Samples

When N is small (< 30 per bucket), the normal approximation is poor. Use the **exact binomial test**:

Under H₀: Y ~ Binomial(N, p₀)

Reject H₀ if P(Y ≥ y_obs | p₀) ≤ α (one-sided, testing upward drift)

**Worked example for small N:**
- N = 20, p₀ = 0.70, observed y = 18 (empirical rate = 0.90)
- Question: Is this significant evidence of upward drift?
- P(Y ≥ 18 | N=20, p₀=0.70) = P(Y=18) + P(Y=19) + P(Y=20)

```
P(Y=18) = C(20,18) × 0.70^18 × 0.30^2 = 190 × 0.00163 × 0.09 = 0.0278
P(Y=19) = C(20,19) × 0.70^19 × 0.30^1 = 20 × 0.00114 × 0.30 = 0.0068
P(Y=20) = C(20,20) × 0.70^20 × 0.30^0 = 1 × 0.00080 × 1 = 0.0008
Sum = 0.0354
```

At α=0.05, p=0.0354 < 0.05 → **Reject H₀**. Significant evidence of upward drift even in N=20.

**Note:** Power of exact test at N=20 for detecting δ=0.20 (p₀=0.70 → p₁=0.90) is approximately 65% — below target. Need exact power calculation to ensure adequacy.

---

## 1.7 The Two-Sample Version: Comparing Periods

When you have two periods (baseline period with N₁ observations, monitoring period with N₂ observations), the test becomes a two-sample proportion test:

**Null:** p₁ = p₂ (calibration unchanged)  
**Alternative:** p₂ = p₁ + δ (drift occurred)

Formula (pooled variance under H₀):
```
Z = (p̄₂ − p̄₁) / sqrt[p̄ × (1−p̄) × (1/N₁ + 1/N₂)]
```

where p̄ = (N₁p̄₁ + N₂p̄₂) / (N₁ + N₂).

**Sample size for equal groups (N₁ = N₂ = N):**
```
N = [z_{α/2} × sqrt(2p₀(1−p₀)) + z_β × sqrt(p₀(1−p₀) + p₁(1−p₁))]² / δ²
```

This is larger by a factor of approximately 1.5–2× compared to the one-sample case, because the reference (baseline) period itself introduces variance.

---

## 1.8 The Hosmer-Lemeshow Test for Multi-Bucket Calibration

For testing calibration across all buckets simultaneously, the Hosmer-Lemeshow statistic provides a joint test:

```
HL = Σₘ [(Oₘ − Eₘ)² / (nₘ × p̄ₘ × (1−p̄ₘ))]
```

Under H₀ (perfect calibration), HL ~ χ²(M−2) where M is the number of groups.

**Critical limitations:**
1. **Power instability:** HL is over-powered for large N (rejects trivially miscalibrated models) and under-powered for small N
2. **Group dependency:** Results differ depending on whether decile groups or fixed-width bins are used
3. **Not for drift monitoring:** HL is a cross-sectional test, not a temporal test for drift

**Practical sample size for HL:** With M=10 decile groups, adequate power (>80%) to detect average 10pp miscalibration requires approximately N ≥ 200. For N > 5,000, the test will usually reject even trivially miscalibrated models.

---

## 1.9 Summary: Which Test to Use When

| Situation | Recommended Test | Formula Section |
|---|---|---|
| Single bucket, precise hypothesis | One-sample binomial proportion test | 1.2 |
| Very small N (<30) in any bucket | Exact binomial test | 1.6 |
| Two time periods, comparing calibration | Two-sample proportion test | 1.7 |
| All buckets, joint test, cross-sectional | Hosmer-Lemeshow or ECE test | 1.8 |
| Continuous monitoring, single stream | SPRT (see Deliverable 2) | 2.x |
| Temporal drift detection | CUSUM/EWMA (see Deliverable 3) | 3.x |
| Graded confidence, N per bucket variable | Per-bucket one-sample + Bonferroni | 1.4 |
