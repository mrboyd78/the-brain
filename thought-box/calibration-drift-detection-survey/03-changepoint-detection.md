# Deliverable 3: Change-Point Detection — CUSUM and EWMA for Calibration Drift

**Research question:** For calibration that was correct but drifts over time, what is the detection delay for CUSUM and EWMA under standard parameterizations?

---

## 3.1 Why Change-Point Methods for Calibration?

SPRT and GSD are designed to test whether calibration has *ever* drifted from a fixed baseline — they don't model the temporal structure of when the drift occurred. **Change-point detection methods** are specifically designed for a different scenario:

- The system was initially well-calibrated
- At some unknown moment τ, calibration began to drift
- We want to detect the drift as quickly as possible after it occurs

This maps more naturally to production monitoring, where the question is not "has this system ever been miscalibrated?" but "has it started to become miscalibrated recently?"

---

## 3.2 CUSUM: Cumulative Sum Chart

### 3.2.1 The Standard CUSUM Formulation

For binary calibration data, define the residual at time i as:

```
eᵢ = yᵢ − p̂ᵢ
```

where yᵢ ∈ {0,1} is the observed outcome and p̂ᵢ is the declared confidence. Under perfect calibration, E[eᵢ] = 0.

The **two-sided tabular CUSUM** maintains:

```
C⁺ₙ = max(0, C⁺ₙ₋₁ + (eₙ − k))    [upward CUSUM: detecting over-confidence]
C⁻ₙ = max(0, C⁻ₙ₋₁ − (eₙ + k))    [downward CUSUM: detecting under-confidence]
```

Where **k** is the reference value (slack) and **h** is the decision interval (control limit).

**Signal:** Trigger alarm when C⁺ₙ > h or C⁻ₙ > h.

### 3.2.2 Parameter Choice

For calibration monitoring, the standard parameterization (Montgomery, 2020) is:

- **k = δ/2**: Set reference to half the drift you want to detect
  - For δ = 0.10: k = 0.05
  - For δ = 0.20: k = 0.10
  - For δ = 0.30: k = 0.15

- **h**: Set to achieve desired ARL₀ (average run length under H₀)
  - For ARL₀ = 370 (commonly used in process control): h ≈ 4.0 to 5.0
  - For ARL₀ = 200 (more sensitive): h ≈ 2.9 to 3.5
  - For ARL₀ = 500 (conservative): h ≈ 5.5 to 6.5

**Standard parameterization for calibration monitoring:**
- k = δ/2 (tuned to drift of interest)
- h = 4.5 (ARL₀ ≈ 370 for standard normal; adjustments needed for binomial)

### 3.2.3 ARL for the Calibration CUSUM

For binary calibration residuals with variance σ² ≈ p₀(1−p₀) = 0.21 at p₀=0.70:

The standardized CUSUM uses normalized residuals:
```
zᵢ = eᵢ / σ = (yᵢ − p̂ᵢ) / sqrt(p̂ᵢ(1−p̂ᵢ))
```

Then: k* = (δ/σ)/2 and h* = h/σ

**ARL₁ for CUSUM (detection delay after shift of magnitude δ in units of σ):**

For k* = 0.5 (CUSUM designed for 1σ shift), h* = 4.77 (ARL₀ ≈ 370):

| Shift Size δ/σ | ARL₁ (expected detection delay) |
|---|---|
| 0.25σ | ~139 |
| 0.50σ | ~31 |
| 1.00σ | ~10.4 |
| 1.50σ | ~6.3 |
| 2.00σ | ~4.8 |
| 3.00σ | ~3.5 |

### 3.2.4 Converting to Calibration Terms

For the calibration problem with p₀ = 0.70, σ = 0.458:

| Drift δ (pp) | δ/σ | ARL₁ (expected observations after drift to detection) |
|---|---|---|
| 0.05 (5 pp) | 0.109 | ~500 |
| 0.10 (10 pp) | 0.218 | ~160 |
| 0.15 (15 pp) | 0.328 | ~85 |
| 0.20 (20 pp) | 0.437 | ~50 |
| 0.30 (30 pp) | 0.655 | ~27 |

**Interpretation:** If calibration drifts by 20 percentage points at time τ, the CUSUM will raise an alarm (on average) approximately **50 observations after the drift began**. This is the *detection delay*.

For p₀ = 0.50 (σ = 0.500, slightly larger):

| Drift δ (pp) | δ/σ | ARL₁ |
|---|---|---|
| 0.10 (10 pp) | 0.200 | ~175 |
| 0.20 (20 pp) | 0.400 | ~57 |
| 0.30 (30 pp) | 0.600 | ~30 |

### 3.2.5 CUSUM Worked Example

**Setup:** Monitoring a 70%-confidence bucket. Historical calibration: 70% correct accuracy. A drift begins at observation τ=100, causing true accuracy to shift to 85% (δ=0.15).

CUSUM parameters: k = 0.075 (= δ/2 = 0.15/2), h = 4.5

**Simulation trace (conceptual):**
- Observations 1–100: Random residuals around 0 (calibrated). C⁺ wanders near 0.
- Observations 101–110: Positive residuals (+0.15 on average). C⁺ grows by ≈0.075/observation (after subtracting k).
- Observation ~163: C⁺ ≈ 4.5 → **ALARM**. Average detection delay ≈ 63 observations after drift start, consistent with ARL₁ ≈ 57–85 for 15pp shift at p₀=0.70.

---

## 3.3 EWMA: Exponentially Weighted Moving Average

### 3.3.1 The EWMA Statistic

The EWMA monitors the calibration residual stream with exponential weighting:

```
Zₙ = λ × eₙ + (1−λ) × Zₙ₋₁
```

Where:
- eₙ = yₙ − p̂ₙ (calibration residual)
- λ ∈ (0, 1] is the smoothing parameter
- Z₀ = 0 (initialized at zero, the target under H₀)

**Control limits:**

```
UCL = +L × σ × sqrt[λ/(2−λ)]
LCL = −L × σ × sqrt[λ/(2−λ)]
```

Where σ = sqrt(p₀(1−p₀)) and L is chosen to achieve the desired ARL₀.

**Signal:** Alarm when Zₙ > UCL (over-confidence) or Zₙ < LCL (under-confidence).

### 3.3.2 EWMA Parameter Choice

**Smoothing parameter λ:**
- λ = 0.05–0.10: Detects very small persistent shifts; slow to react to sudden shifts
- λ = 0.15–0.25: Balanced — good sensitivity to small-to-moderate shifts **(recommended for calibration monitoring)**
- λ = 0.40–0.50: Approaches Shewhart chart; responsive to large single-observation deviations
- λ = 1.0: Pure Shewhart chart (no memory)

**Control limit multiplier L for ARL₀:**
- For ARL₀ = 370: L ≈ 3.00 (λ=0.10), L ≈ 2.98 (λ=0.20), L ≈ 2.96 (λ=0.40)
- For ARL₀ = 200: L ≈ 2.60 (λ=0.10), L ≈ 2.58 (λ=0.20)
- For ARL₀ = 500: L ≈ 3.28 (λ=0.10)

**Standard parameterization (λ=0.20, L=3.00):**
- ARL₀ ≈ 370 (false alarm every 370 observations on average under null)
- Highly sensitive to shifts in 0.5σ to 1.5σ range

### 3.3.3 EWMA ARL₁ Table

For λ=0.20, L=3.00, ARL₀=370:

| Shift Size δ/σ | ARL₁ (EWMA) | ARL₁ (CUSUM, k=0.5) | EWMA advantage |
|---|---|---|---|
| 0.25σ | ~80 | ~139 | EWMA 43% faster |
| 0.50σ | ~27 | ~31 | EWMA 13% faster |
| 1.00σ | ~10 | ~10.4 | ≈Equal |
| 1.50σ | ~6.5 | ~6.3 | ≈Equal |
| 2.00σ | ~5.0 | ~4.8 | ≈Equal |
| 3.00σ | ~3.3 | ~3.5 | CUSUM slightly faster |

**Key finding:** EWMA outperforms CUSUM for *small* systematic shifts (the gradual drift case), while CUSUM performs similarly or slightly better for *large* sudden shifts. Since calibration drift tends to be gradual, **EWMA with λ=0.15–0.25 is generally preferred for calibration monitoring**.

---

## 3.4 Calibration-Specific CUSUM Adaptation

### 3.4.1 The Bernoulli CUSUM

For binary outcomes (not normally distributed), the exact CUSUM uses the log-likelihood ratio directly:

```
C⁺ₙ = max(0, C⁺ₙ₋₁ + log[p₁(yₙ)/p₀(yₙ)])
```

where p₁(y) and p₀(y) are the Bernoulli likelihoods.

This simplifies to:
- When yₙ = 1: increment = log(p₁/p₀)
- When yₙ = 0: increment = log((1−p₁)/(1−p₀))

This is identical to the SPRT log-likelihood ratio update. The CUSUM is therefore the running maximum of the SPRT statistic — and CUSUM signals when the maximum exceeds a threshold.

**Advantage of Bernoulli CUSUM:** No Gaussian approximation error; exact for binary outcomes. **Recommended over Gaussian CUSUM for small p (< 0.20) or large p (> 0.80)** where the normal approximation is poor.

### 3.4.2 The Adaptive Calibration Residual CUSUM

For a system with *variable* declared confidence (different predictions at different confidence levels), a better residual is:

```
eᵢ = yᵢ − p̂ᵢ                   [raw miscalibration residual]
```

This has mean 0 under perfect calibration and variance p̂ᵢ(1−p̂ᵢ) — which varies by p̂ᵢ.

For the CUSUM, normalize:
```
zᵢ = (yᵢ − p̂ᵢ) / sqrt(p̂ᵢ(1−p̂ᵢ))   [variance-stabilized residual]
```

The variance-stabilized CUSUM (also called the "score-based CUSUM") has approximately unit variance under null calibration regardless of the declared confidence, enabling uniform control limits.

---

## 3.5 Page's Test

### 3.5.1 Connection to CUSUM

**Page's test** (Page, 1954) is formally identical to the one-sided CUSUM but was derived for detecting an upward shift in the mean of a normal sequence. For calibration monitoring, Page's test can be applied to the running calibration gap.

**Page's statistic:**
```
Sₙ = max(0, Sₙ₋₁ + gₙ)

where gₙ = eₙ − k  (calibration residual minus the slack)
```

This is exactly the tabular CUSUM C⁺. "Page's test" and "CUSUM" are effectively the same procedure in the calibration context.

### 3.5.2 Page's Test for the Brier Score

An alternative to monitoring binary residuals directly is to monitor the **Brier score decomposition**:

```
BSᵢ = (yᵢ − p̂ᵢ)² 
```

E[BSᵢ] under calibration: p̂ᵢ(1−p̂ᵢ) + [calibration error]²

Monitoring the Brier score with CUSUM/EWMA detects general deterioration in probabilistic forecasting, including both calibration and resolution degradation.

---

## 3.6 Detection Delay Summary Table

For practical reference — expected delay in observations from drift onset to alarm signal:

### Table 3.1: Detection Delay by Method and Drift Magnitude (p₀=0.70, σ=0.458)

| Method | Params | δ=0.10 (10pp) | δ=0.15 (15pp) | δ=0.20 (20pp) | δ=0.30 (30pp) |
|---|---|---|---|---|---|
| CUSUM | k=0.05, h=4.5 | ~210 | ~120 | ~80 | ~45 |
| CUSUM | k=0.10, h=4.5 | ~160 | ~85 | ~50 | ~27 |
| CUSUM | k=0.15, h=4.5 | ~190 | ~95 | ~58 | ~30 |
| EWMA | λ=0.10, L=3.0 | ~95 | ~52 | ~35 | ~18 |
| EWMA | λ=0.20, L=3.0 | ~130 | ~65 | ~40 | ~22 |
| EWMA | λ=0.10, L=2.6 | ~60 | ~35 | ~23 | ~13 |
| SPRT (sequential) | δ=0.10, α=0.05 | ~100 | ~55 | ~33 | ~18 |

*Note: ARL₀ is ≈370 for L=3.0 rows, ≈200 for L=2.6 rows. Reducing ARL₀ speeds detection but increases false alarm rate.*

### Table 3.2: ARL₀ (false alarm frequency) for the same configurations

| Method | Params | ARL₀ | Expected false alarms per 1,000 obs |
|---|---|---|---|
| CUSUM | k=0.05, h=4.5 | ~370 | 2.7 |
| CUSUM | k=0.10, h=4.5 | ~370 | 2.7 |
| EWMA | λ=0.10, L=3.0 | ~370 | 2.7 |
| EWMA | λ=0.10, L=2.6 | ~200 | 5.0 |
| EWMA | λ=0.20, L=2.6 | ~200 | 5.0 |

---

## 3.7 Choosing Between CUSUM and EWMA for Calibration

| Criterion | CUSUM (k tuned to δ) | EWMA (λ=0.15–0.20) |
|---|---|---|
| **Best for gradual drift** | Moderate | **Better** |
| **Best for sudden shift** | **Better** | Moderate |
| **Requires knowing δ** | Yes (to set k) | No |
| **Computational complexity** | Minimal | Minimal |
| **Memory of past states** | Yes (C resets on alarm) | Yes (exponential decay) |
| **Multiple confidence levels** | Can be pooled | Can be pooled |
| **Interpretability** | Moderate | High (smoothed calibration gap) |
| **Autocorrelated data** | Requires adjustment | More robust |

**Recommendation for calibration drift monitoring:**
- **Primary: EWMA with λ=0.15 to 0.20, L=3.0** — robust to gradual drift, no need to specify exact drift size, easy to interpret (the EWMA value is a smoothed calibration gap)
- **Secondary/complementary: Two-sided CUSUM with k = δ_expected/2** — sharper detection if you have a strong prior about expected drift magnitude
- **Run both in parallel** if the stakes are high; alarm when either triggers

---

## 3.8 Practical Implementation Notes

### 3.8.1 Warm-Up Period

Both CUSUM and EWMA should have a **warm-up period** of at least 50–100 observations before the control chart is activated, during which baseline calibration is confirmed. This prevents false alarms from the initialization transient.

### 3.8.2 Resetting After Alarm

When an alarm is triggered:
1. Investigate the cause
2. If genuine drift: recalibrate the system, then **reset the CUSUM to C=0** (or EWMA to Z=0) and resume monitoring
3. If false alarm: document and continue without resetting (resetting after every false alarm inflates subsequent false alarm rates)

### 3.8.3 Adaptive Control Limits

For non-stationary environments where the baseline p₀ changes over time (e.g., due to changing prediction distributions), the control limits must be updated:

```
σₙ = sqrt(p̂ₙ(1−p̂ₙ))   [use current predicted probability as the null variance]
UCLₙ = L × σₙ × sqrt[λ/(2−λ)]
```

This **adaptive EWMA** is more robust in non-stationary environments but introduces additional sensitivity to the estimate of σₙ.
