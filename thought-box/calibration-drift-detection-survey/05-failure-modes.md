# Deliverable 5: Failure Modes — False Alarms, Missed Drift, and Pathological Regimes

**Research question:** When do these methods produce false alarms or miss true drift? Identify the regimes where each failure mode is dominant.

---

## 5.1 Taxonomy of Failure Modes

All calibration monitoring methods can fail in two fundamental directions:

| Failure Type | Technical Name | Consequence |
|---|---|---|
| **False alarm** | Type I error / false positive | Unnecessary investigation, system disruption, calibration "fixes" that introduce real problems |
| **Missed drift** | Type II error / false negative | Continued operation with miscalibrated system, undetected harm |

Beyond these, there are **methodological failures** — cases where the test gives formally correct statistical output but the output does not reflect the underlying calibration reality:
- **Wrong bucket attribution:** Drift detected in B4 when the real drift is in B3 due to binning artifacts
- **Magnitude hallucination:** Detected drift is 20pp but true drift is 7pp (type-M error)
- **Direction error:** Test reports over-confidence when system is under-confident (type-S error)
- **Stale baseline:** The "null" calibration is wrong because baseline period itself was miscalibrated

---

## 5.2 Regime 1: Low Base Rate Outcomes

### 5.2.1 The Problem

When true events are rare (e.g., base rate p ≈ 0.05), binary calibration monitoring faces a severe signal-to-noise problem:

**Observed outcome variance = p(1−p) ≈ 0.05 × 0.95 = 0.0475**

This is the minimum variance a Bernoulli variable can have. However, when the system rarely assigns high confidence (most predictions are in the 10–20% range), bucket B5 (75–100% confidence) contains very few observations, and those few outcomes are almost always 0 (incorrect) due to the low base rate.

**Consequence of low base rate:**

1. **Small effective N in high-confidence buckets:** If the system assigns 80% confidence only 5% of the time, to accumulate 200 observations in B5 requires 4,000 total predictions.

2. **Systematic under-performance in high-confidence regions:** A system that assigns 80% confidence to rare-event predictions will almost always appear miscalibrated (it will be), but the direction and magnitude of calibration error are difficult to separate from base rate uncertainty.

3. **The "calibration impossible" regime:** When the true event rate is p = 0.02, a 5% confidence assignment and a 2% confidence assignment are practically indistinguishable with N < 1,000.

### 5.2.2 Quantified Impact of Low Base Rate on Required N

For detecting 10pp drift at 80% power, α=0.05:

| Base rate p₀ | N required (10pp drift) | N required (20pp drift) |
|---|---|---|
| 0.50 (balanced) | 338 | 85 |
| 0.20 | 224 | 56 |
| 0.10 | 130 | 33 |
| 0.05 | 71 | 18 |
| 0.02 | 30 | 8 |

**Apparent good news:** Low base rates *reduce* required N! This is because the variance is low (p(1−p) is small), and a 10pp drift represents a huge proportional change.

**The hidden problem:** Low base rates create **severe type-M error risks**. A detected "10pp drift" from p₀=0.02 to p₁=0.12 at N=30 is based on changing from ~0–1 observed successes to ~3 successes — extreme sensitivity to individual observations.

### 5.2.3 The Rare Outcome Paradox

In low base-rate regimes, the CUSUM and EWMA behave pathologically:

**EWMA at low base rate (p₀=0.05, λ=0.20):**
- Most observations: yᵢ=0, eᵢ = 0 − 0.05 = −0.05. EWMA pulls toward −0.05 each step.
- Rare "hit" (yᵢ=1): eᵢ = 1 − 0.05 = 0.95. EWMA jumps to 0.20 × 0.95 + 0.80 × Z_{prev} — a large spike.
- The EWMA for rare events looks like a baseline bias with rare large spikes, making it hard to distinguish systematic drift from random runs of good/bad luck.

**Mitigation for low base rate environments:**
1. Use **arcsine stabilization**: replace residuals with 2×arcsin(√p) transformation before applying EWMA/CUSUM — equalizes variance across base rates
2. Require **longer warm-up periods**: minimum 200 observations (not 50) before activating monitoring
3. Use **posterior predictive monitoring** (Bayesian): model the Dirichlet-multinomial posterior over bucket frequencies, which naturally handles sparse buckets

---

## 5.3 Regime 2: Non-Stationary Environments (Concept Drift)

### 5.3.1 The Problem

All standard calibration monitoring methods assume **stationarity**: the underlying process generating outcomes is fixed, and calibration is the only thing that can change. In real environments:

- **Covariate shift:** The distribution of inputs changes (e.g., different types of questions)
- **Concept drift:** The true relationship between inputs and outcomes changes (e.g., world events change what was previously a reliable pattern)
- **Label shift:** The base rate of positive outcomes changes

**The danger:** CUSUM/EWMA/SPRT will alarm on any systematic deviation from the null, including concept drift that the reasoner correctly responds to. A system that *appropriately* increases its confidence because new information changed the base rate will appear miscalibrated by comparison to its historical baseline.

### 5.3.2 Distinguishing Calibration Drift from Concept Drift

| Observation | Likely Cause |
|---|---|
| All confidence buckets show drift in the same direction | Concept drift (base rate change) |
| One or two buckets show drift while others are stable | Calibration drift (specific confidence range affected) |
| ECE increases but calibration plot shows roughly parallel shift | Concept drift |
| ECE increases and calibration slope changes | Calibration drift |
| Drift appears abrupt (CUSUM signals within 20 obs) | Structural shift, possibly concept drift |
| Drift appears gradual (CUSUM signals after 100+ obs) | Slow calibration erosion |

### 5.3.3 The False Alarm Inflation from Non-Stationarity

**Quantified impact:** If the true base rate shifts by 5pp (from p=0.70 to p=0.75) while declared confidence remains at p₀=0.70:

- The EWMA will see systematic positive residuals of 0.05 per observation
- ARL₁ for 5pp drift at λ=0.20, ARL₀=370: ≈500 observations to alarm
- But this alarm is a "true alarm" for concept drift AND a "false alarm" for miscalibration drift

**The fundamental identification problem:** Without independent knowledge of whether the true base rate has changed, it is impossible to determine from observation data alone whether an alarm represents calibration failure or appropriate response to a changed world.

**Mitigation strategies:**

1. **External benchmark tracking:** Compare system predictions against an independent benchmark (another forecaster or a base-rate model). If the benchmark also shows the "drift," it's likely concept drift.

2. **Stripped calibration:** Separate the calibration signal from the base rate signal using the calibration slope:
   ```
   Calibration slope β = Cov(y, p̂) / Var(p̂)
   Calibration intercept α = ȳ − β × p̄
   ```
   Under perfect calibration: β=1, α=0. Monitor β and α separately: drift in α alone suggests base rate shift; drift in β suggests calibration scaling error.

3. **Rolling reference period:** Use a rolling 60-day baseline instead of a fixed historical baseline to allow the null hypothesis to adapt to genuine base rate changes.

4. **Stratified drift analysis:** Test separately by domain, question type, or context. If drift is universal, it's likely concept drift; if domain-specific, it's likely calibration failure in that domain.

---

## 5.4 Regime 3: Clustered/Dependent Observations

### 5.4.1 The Problem

All tests (SPRT, CUSUM, fixed-N) assume **independent observations**. In reasoning systems:

- **Sequential decisions on the same case:** Multiple predictions about related aspects of a case are correlated
- **Learning effects:** Later decisions may be influenced by feedback from earlier ones
- **Cluster effects:** Decisions in the same domain on the same day may share common contextual factors
- **Autocorrelation:** Performance fluctuates in "good days" and "bad days" patterns

**Impact on false alarm rate:** When observations are positively correlated (ρ > 0), the effective sample size is:

```
N_effective = N / (1 + (N−1) × ρ)
```

For ρ = 0.10 and N = 200:
```
N_eff = 200 / (1 + 199 × 0.10) = 200 / 20.9 ≈ 9.6
```

The effective sample size is only 10 — vastly smaller than the 200 nominal observations. All CIs and p-values calculated assuming N=200 will be dramatically overconfident. **This is the single most dangerous failure mode in practice.**

### 5.4.2 Quantified Impact on False Alarm Rate

For the EWMA with λ=0.20, L=3.0, nominal ARL₀=370:

| Intraclass Correlation ρ | True ARL₀ | False alarms per 1,000 obs |
|---|---|---|
| 0.00 (independent) | 370 | 2.7 |
| 0.05 | ~150 | 6.7 |
| 0.10 | ~80 | 12.5 |
| 0.20 | ~40 | 25.0 |
| 0.50 | ~15 | 67.0 |

**At ρ=0.10**, the false alarm rate is 4.6× higher than expected. The system would trigger weekly false alarms even when perfectly calibrated.

### 5.4.3 Mitigation for Dependent Observations

1. **Independence sampling:** Select a stratified random sample of 1 decision per case/context per day for monitoring purposes (use remaining decisions for other analyses but not calibration monitoring)

2. **Cluster-robust standard errors:** Replace σ² = p₀(1−p₀) with the cluster-robust variance estimate:
   ```
   σ²_robust = p₀(1−p₀) × [1 + (cluster_size − 1) × ρ̂]
   ```

3. **Adjust control limits:** Inflate control limits by the design effect DE:
   ```
   DE = 1 + (m̄ − 1) × ρ̂   [where m̄ = average cluster size]
   UCL_adjusted = UCL × sqrt(DE)
   ```

4. **Run EWMA on batch averages:** Rather than per-observation updates, update the EWMA weekly with the batch mean — this reduces autocorrelation across weeks.

---

## 5.5 Regime 4: Systematic Binning Artifacts (ECE and Reliability Diagram Problems)

### 5.5.1 The Problem

ECE and reliability diagrams depend on binning decisions that are arbitrary:

**Equal-width bins (0–10%, 10–20%, ..., 90–100%):**
- Sparse high-confidence bins when system rarely expresses high confidence
- Artificially low ECE when many bins are empty (weighted by n/N, empty bins contribute nothing)

**Equal-frequency bins (quantile bins):**
- Bins contain equal numbers of observations
- Bin boundaries shift as data accumulates — change-point detection on ECE is contaminated by bin boundary changes

**The ECE bias problem (Nixon et al., 2019):** With M bins and N observations:
```
E[ECE_observed] ≈ ECE_true + Σₘ (1/2nₘ) × sqrt(2pₘ(1−pₘ)/π)
```

The second term is an upward bias (ECE is always overestimated in finite samples). With M=10 bins and N=100 (10 observations per bin), the bias term is approximately:

```
Bias ≈ 10 × (1/20) × sqrt(2 × 0.5 × 0.5 / π) ≈ 0.5 × 0.399 ≈ 0.02
```

A 2 percentage point **upward bias** in ECE even when the system is perfectly calibrated. This means ECE < 2pp should not be treated as evidence of miscalibration.

### 5.5.2 The Adaptive Calibration Error (ACE) Alternative

The **Adaptive Calibration Error (ACE)** uses equal-frequency bins adaptively sized to ensure each bin has at least 10 observations. This reduces but does not eliminate binning artifact issues.

**Better approach:** Use the kernel-based calibration estimator (Popordanoska et al., 2022):

```
ECE_kernel = (1/N) × Σᵢ |p̂ᵢ − ĝ(p̂ᵢ)|
```

where ĝ(p) is the kernel regression estimate of E[Y|p̂=p]. This does not require binning and has lower bias.

---

## 5.6 Regime 5: The "Winner's Curse" in Calibration Alarms

### 5.6.1 Type-M Error in Practice

Gelman & Carlin (2014) demonstrate that when power is less than 100%, any statistically significant detection understates the uncertainty in the detected effect size.

**For calibration monitoring, the type-M error works as follows:**

Suppose true drift is δ=0.08 (8pp) and we're using a test designed for δ=0.10 with N=338 (80% power at δ=0.10). This means:
- Power at true δ=0.08: approximately 50% (underpowered for the actual effect)
- When we do detect significance, the observed p̄ − p₀ will average to approximately 0.115 (not 0.08)
- **Exaggeration ratio ≈ 1.44×** (detected value is 44% larger than true value)

**Policy implication:** When a calibration audit flags a bucket as significantly miscalibrated with a borderline p-value (0.01 < p < 0.05), the *reported* calibration gap should be treated as an *overestimate* of the true gap. Do not immediately recalibrate to the detected magnitude — apply a shrinkage estimate:

```
δ̂_shrunk = δ̂_observed × [power at δ̂_observed / power at δ̂_observed + sampling_noise_factor]
```

As a quick rule: if detected at borderline significance, assume true gap ≈ 60–70% of the detected gap.

---

## 5.7 Regime 6: Looking Forward — Outcome Lag Bias

### 5.7.1 The Problem of Selective Outcome Availability

If monitoring uses *only* resolved outcomes, and resolution is correlated with outcome valence, the monitoring sample is selectively biased.

**Example:** If a reasoning system makes predictions about court cases, cases that settle quickly (out-of-court) resolve sooner. If the system is better calibrated for settlement-prone cases vs. trial cases, using only early-resolved outcomes will show artificially positive calibration relative to the full distribution.

**Detection:** Check whether the distribution of declared confidence differs between resolved vs. unresolved outcomes at any monitoring timestamp. If p̂ distribution differs, selective resolution bias is present.

**Mitigation:** Use inverse probability of resolution weighting (IPRW) to reweight observations by the probability of being resolved by the monitoring date.

---

## 5.8 Regime 7: SPRT Failure Modes

### 5.8.1 The Composite Alternative Problem

SPRT requires specifying both H₀ and H₁ exactly. In practice, the true drift magnitude δ is unknown.

**When SPRT is parameterized for δ=0.10 but true drift is δ=0.05:**
- SPRT expects to reach boundary quickly but the drift is too small
- Result: very long run without boundary crossing — the test "gets stuck"
- ESN explodes: instead of ~100, might require 500+ before decision

**Solution:** Use mSPRT (mixture SPRT) with a prior distribution over δ. For calibration monitoring, a half-normal prior N(0, 0.15) on |δ| provides robustness to a range of drift magnitudes.

### 5.8.2 The Sequential Peeking Trap

SPRT formally controls Type I error when applied as designed. **The trap:** practitioners who use the SPRT statistic as a "score" but apply ad hoc stopping rules (e.g., "let's stop when it looks like it's heading for the boundary") do not maintain the α guarantee.

**Rule:** The SPRT provides valid error control ONLY if:
1. Stopping occurs exactly when the boundary is crossed (or at a pre-specified maximum N)
2. No informal intermediate decisions are made based on the current Λₙ value
3. The null and alternative are specified before data collection begins

---

## 5.9 Failure Mode Summary Matrix

| Method | Primary Failure Mode | Secondary | Pathological Regime |
|---|---|---|---|
| Fixed-N proportion test | Underpowered at low N; overpowered at high N | Multiple testing inflation | Very large or very small N |
| SPRT | Wrong δ specification | Ignoring optional stopping rules | Composite alternative |
| CUSUM | Designed for wrong shift size | ARL degrades fast if k is off | Autocorrelated residuals |
| EWMA | Slow to detect sudden large shifts | Autocorrelation sensitivity | Non-stationary environments |
| ECE / HL test | Binning artifacts | Biased at finite N | Low N per bin |
| Sequential BF | BF threshold ≠ frequentist α | Prior sensitivity | Very strong priors |

---

## 5.10 Regime Identification Flowchart

```
Is the outcome base rate < 10% or > 90%?
  → If YES: Regime 1 (low base rate). Use exact binomial; avoid ECE; 
     require 300+ obs per bucket; apply arcsine transformation.

Are observations clustered (multiple decisions per case/day)?
  → If YES: Regime 3 (dependence). Apply design effect correction; 
     use independent sampling or cluster-robust SEs.

Does the outcome base rate change over time?
  → If YES: Regime 2 (non-stationarity). Use rolling baseline; 
     track calibration slope separately; watch for universal vs. 
     bucket-specific drift.

Is N < 50 per bucket?
  → If YES: Use exact tests only; suppress EWMA/CUSUM; report CI not p-value.

Are p-values borderline (0.01 < p < 0.05)?
  → Apply Gelman-Carlin correction: true gap ≈ 60-70% of detected gap.
     Require replication before action.

Are bins sparse (< 10 obs per bin)?
  → Regime 4 (ECE artifact). Use kernel ECE or pool adjacent bins.
```
