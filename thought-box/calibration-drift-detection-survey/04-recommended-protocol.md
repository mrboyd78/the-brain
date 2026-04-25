# Deliverable 4: Recommended Monitoring Protocol

**Research question:** A concrete, implementable miscalibration-monitoring design for a reasoning system that logs one decision per entry, with declared-confidence and in-hindsight-correctness fields.

---

## 4.1 System Requirements

### 4.1.1 The Minimum Necessary Data Schema

Each logged decision entry must contain at least:

```json
{
  "decision_id":     "uuid",
  "timestamp":       "ISO-8601 datetime",
  "confidence":      0.75,        // declared probability (0.0 to 1.0)
  "outcome":         1,           // resolved truth value: 1=correct, 0=incorrect
  "outcome_timestamp": "ISO-8601" // when the outcome was observed
}
```

**Optional enrichment fields:**
```json
{
  "domain":          "medical",   // for stratified monitoring
  "context_id":      "case-123",  // for dependency detection
  "model_version":   "v2.3.1",   // for version-specific monitoring
  "outcome_lag_days": 3          // for lag-adjustment analysis
}
```

### 4.1.2 System Assumptions

This protocol assumes:
1. Outcomes resolve within a predictable lag (handled in §4.3)
2. Decisions are approximately independent (no strong sequential correlations)
3. The system has a stable baseline calibration period of at least 200 observations
4. Declared confidence values are continuous or discretized into at most 10 levels

---

## 4.2 Bucket Assignment and Minimum Data Requirements

### 4.2.1 Confidence Bucket Definition

Assign each prediction to one of 5 calibration buckets:

| Bucket ID | Confidence Range | Center p₀ | Description |
|---|---|---|---|
| B1 | [0.00, 0.25) | 0.125 | Low confidence |
| B2 | [0.25, 0.45) | 0.350 | Moderate-low |
| B3 | [0.45, 0.55) | 0.500 | Near-uncertain |
| B4 | [0.55, 0.75) | 0.650 | Moderate-high |
| B5 | [0.75, 1.00] | 0.875 | High confidence |

**Rationale for 5 buckets:** Fewer buckets → less precision in detecting which confidence range is miscalibrated; more buckets → more data required per bucket. Five is the practical sweet spot for systems generating 100–1,000 decisions per week.

### 4.2.2 Minimum Viable Population per Bucket

Before any monitoring signal is meaningful, each bucket needs a **minimum population**:

| Monitoring goal | Min N per bucket | Justification |
|---|---|---|
| Detect 30pp drift (casual monitoring) | 25 | Exact binomial at N=20 has 65% power |
| Detect 20pp drift (standard) | 60 | Provides 80% power after Bonferroni correction |
| Detect 10pp drift (precise monitoring) | 200 | Minimum for reliable detection |
| High-precision audit | 500 | Covers the full range of drift magnitudes |

**Action:** Do not activate calibration monitoring for a bucket until it has accumulated at least 50 resolved outcomes.

---

## 4.3 Handling Resolution Lag

Many reasoning systems have prediction-to-resolution lags (days, weeks, months). This creates a processing pipeline:

```
[Prediction logged] → [Lag period] → [Outcome resolved] → [Eligible for monitoring]
```

**Protocol:**
1. Log predictions in a **prediction queue** immediately when made
2. Move to **monitoring-eligible pool** only when outcome is resolved
3. Track lag distribution: if 90th percentile lag is L days, report monitoring coverage = decisions made ≥ L days ago
4. **Never** use unresolved outcomes in calibration computations (even if proxy outcomes are available — this introduces systematic bias)

**Lag-adaptive EWMA:** In high-lag environments, use a **batch EWMA** that updates the monitoring statistic once per lag period (e.g., weekly), processing all outcomes resolved in the past week as a batch.

---

## 4.4 The Three-Layer Monitoring Stack

The protocol uses three complementary monitoring layers that operate at different timescales:

```
Layer 3: CUSUM/EWMA (continuous, per-observation)    — Fast alarm
Layer 2: Weekly SPRT (accumulate 7 days, then test)  — Moderate precision
Layer 1: Monthly Fixed-N audit (all resolved outcomes) — High precision
```

### 4.4.1 Layer 1: Monthly Fixed-N Audit

**Purpose:** High-precision, comprehensive calibration assessment  
**Cadence:** Monthly (or whenever 500+ new outcomes resolve)  
**Method:** Per-bucket binomial proportion test with Bonferroni correction

**Procedure:**

```
For each bucket Bₖ:
    1. Collect all resolved outcomes in this calendar month
    2. Compute:
       nₖ = count of outcomes in bucket Bₖ
       sₖ = count of correct outcomes (yᵢ=1) in bucket Bₖ
       p̄ₖ = sₖ / nₖ
       p₀ₖ = center confidence for bucket Bₖ

    3. Test at α_bucket = 0.01 (Bonferroni: 0.05/5 buckets):
       z = (p̄ₖ − p₀ₖ) / sqrt(p₀ₖ × (1−p₀ₖ) / nₖ)
       
       Two-sided test: |z| > 2.576 → Flag bucket Bₖ as MISCALIBRATED
       
    4. Report:
       - Calibration gap: p̄ₖ − p₀ₖ  (signed)
       - Confidence interval: p̄ₖ ± 2.576 × sqrt(p̄ₖ(1−p̄ₖ)/nₖ)
       - Sample size: nₖ
       - Power assessment: was nₖ sufficient to detect 10pp drift?
```

**Monthly audit output template:**

```
CALIBRATION AUDIT REPORT — [Month Year]
========================================
Total resolved outcomes: N
Monitoring period: [Start] to [End]

Bucket  | n   | Declared p₀ | Observed | Gap     | z-score | Status
--------|-----|-------------|----------|---------|---------|--------
B1      | 95  | 12.5%       | 14.2%    | +1.7pp  | 0.42    | OK
B2      | 145 | 35.0%       | 36.8%    | +1.8pp  | 0.44    | OK
B3      | 128 | 50.0%       | 42.2%    | −7.8pp  | −1.77   | WATCH*
B4      | 167 | 65.0%       | 74.3%    | +9.3pp  | 2.38    | WATCH*
B5      | 212 | 87.5%       | 79.2%    | −8.3pp  | −3.21   | FLAG ⚠️

Overall ECE: 4.2pp  (prior month: 2.1pp)  ← Trending up, investigate

*WATCH = z > 1.96 but < 2.576 (one-sided significance, not Bonferroni-corrected)
FLAG = Bonferroni-corrected significant at FWER=0.05
```

### 4.4.2 Layer 2: Weekly SPRT

**Purpose:** Faster detection than monthly audits without continuous per-observation overhead  
**Cadence:** Weekly (every Monday, processing prior week's resolved outcomes)  
**Method:** SPRT for each bucket

**SPRT parameter setup (one-time configuration):**

```python
# For each bucket, compute SPRT thresholds
alpha = 0.05      # Type I error
beta = 0.20       # Type II error (power = 80%)
delta = 0.15      # Drift magnitude to detect (15pp — moderate drift)

A = log((1 - beta) / alpha)    # = log(16) ≈ 2.773   [upper: reject H₀]
B = log(beta / (1 - alpha))    # = log(4/19) ≈ −1.558 [lower: accept H₀]
```

**Running the weekly SPRT:**

```python
def update_sprt(p0, p1, prev_lambda, new_outcomes):
    """
    p0: null calibration rate (declared confidence center)
    p1: alternative = p0 + delta
    prev_lambda: accumulated log-likelihood ratio
    new_outcomes: list of (y_i, p_hat_i) for this bucket's weekly outcomes
    """
    lam = prev_lambda
    for y, p_hat in new_outcomes:
        # Use actual declared p_hat (not just bucket center) for precision
        p0_i = p_hat         # null: declared confidence is true accuracy
        p1_i = p_hat + delta # alternative: +15pp drift
        
        if y == 1:
            increment = log(p1_i / p0_i)
        else:
            increment = log((1 - p1_i) / (1 - p0_i))
        
        lam += increment
    
    if lam >= A:
        return lam, "DRIFT_DETECTED"
    elif lam <= B:
        return 0.0, "CALIBRATED"      # reset on acceptance
    else:
        return lam, "CONTINUE"
```

**Decision logic:**
- "DRIFT_DETECTED" → Trigger investigation, escalate to human review, consider recalibration
- "CALIBRATED" → Log as confirmed OK this week, reset SPRT statistic to zero
- "CONTINUE" → Accumulate to next week

**Weekly SPRT reset policy:** When "CALIBRATED" is declared, reset the accumulated statistic to 0 before the next week's analysis. This prevents old evidence from contaminating new monitoring (the SPRT statistic should reflect *current* calibration state, not historical buildup).

### 4.4.3 Layer 3: Continuous EWMA (Per-Observation)

**Purpose:** Fastest possible alarm for large or sudden drifts  
**Cadence:** Updates on every newly resolved outcome  
**Method:** Per-bucket EWMA on calibration residuals

**EWMA configuration:**

```python
class CalibrationEWMA:
    def __init__(self, p0, lambda_=0.15, L=3.0, warmup=50):
        self.p0 = p0
        self.lam = lambda_
        self.L = L
        self.Z = 0.0            # EWMA statistic, initialized at 0
        self.sigma = sqrt(p0 * (1 - p0))
        self.UCL = L * self.sigma * sqrt(self.lam / (2 - self.lam))
        self.LCL = -self.UCL
        self.n_observations = 0
        self.warmup = warmup     # no alarms during warm-up
    
    def update(self, y_observed, p_hat_declared):
        residual = y_observed - p_hat_declared
        self.Z = self.lam * residual + (1 - self.lam) * self.Z
        self.n_observations += 1
        
        if self.n_observations < self.warmup:
            return "WARMUP", self.Z
        
        if self.Z > self.UCL:
            return "DRIFT_UP", self.Z    # Under-confidence (performing better than claimed)
        elif self.Z < self.LCL:
            return "DRIFT_DOWN", self.Z  # Over-confidence (performing worse than claimed)
        else:
            return "IN_CONTROL", self.Z
```

**EWMA alarm thresholds interpretation:**
- "DRIFT_UP": The system is systematically under-confident — true accuracy exceeds declared confidence. (Less harmful but still worth noting)
- "DRIFT_DOWN": The system is over-confident — declared confidence exceeds true accuracy. (Primary concern — calibration erosion)

---

## 4.5 Alert Tiering and Response Procedures

### 4.5.1 Alert Severity Tiers

| Tier | Trigger | Response Time | Action |
|---|---|---|---|
| **Advisory** | Monthly audit: WATCH (|z| > 1.96) | Next review cycle | Monitor more closely, no recalibration |
| **Warning** | Monthly audit: FLAG (Bonferroni) OR Weekly SPRT DRIFT_DETECTED | Within 5 business days | Human review of affected predictions, assess cause |
| **Alarm** | EWMA alarm (Layer 3) | Same day | Immediate human review, consider pausing high-stakes decisions |
| **Critical** | 2+ consecutive months with FLAG in same bucket | Within 24 hours | Mandatory recalibration before resuming full operation |

### 4.5.2 Investigation Checklist

When an alert is triggered:

1. **Data quality check**
   - [ ] Are unresolved outcomes excluded from computation?
   - [ ] Is there unusual clustering of outcomes (dependency violation)?
   - [ ] Any unusual prediction volume change in the flagged bucket?

2. **Drift characterization**
   - [ ] What is the signed direction of drift? (Over vs. under confidence)
   - [ ] Is drift confined to one bucket or across multiple?
   - [ ] When did it start? (Review EWMA time-series for onset)
   - [ ] Is it correlated with any domain, context type, or model version?

3. **Cause determination**
   - [ ] Distribution shift: has the nature of inputs changed?
   - [ ] Base rate shift: has the underlying frequency of correct answers changed?
   - [ ] Model degradation: has model performance dropped independent of calibration?
   - [ ] Systematic bias: is there a consistent over/under-statement in a specific context?

4. **Response selection**
   - [ ] Minor drift (<10pp, single bucket): Log and monitor; no intervention
   - [ ] Moderate drift (10–20pp, 1–2 buckets): Recalibrate via temperature scaling or isotonic regression
   - [ ] Severe drift (>20pp, multiple buckets): Full recalibration, incident review

---

## 4.6 Gelman-Carlin Design Analysis Integration

Before deploying this monitoring protocol, conduct a **design analysis** (Gelman & Carlin, 2014) to assess type-S and type-M error risk:

### 4.6.1 Type-S Error in Calibration Monitoring

**Type-S error:** Detecting "drift" but in the wrong direction (declaring over-confidence when actually under-confident, or vice versa).

For the calibration monitoring context, Type-S error occurs when:
- Sample N is small
- True drift δ is near zero
- A large sampling fluctuation pushes the estimate to the opposite side of p₀

**Type-S probability formula (Gelman & Carlin):**

```
Type-S ≈ P(|Z| > z_α | μ = δ) × P(sign(Z) ≠ sign(δ) | |Z| > z_α)
       = β_power × Φ(−z_α − δ/σ)
       ≈ Φ(z_β − 2δ/σ_effect)   [for small studies]
```

For δ = 0.10, σ_effect = 0.458/√100 ≈ 0.046 (N=100), and z_β = 0.842:

```
Type-S ≈ Φ(0.842 − 2×0.10/0.046) = Φ(0.842 − 4.35) = Φ(−3.51) ≈ 0.0002
```

At N=100, the type-S probability is negligible for a 10pp drift. But at N=20:

```
σ_effect = 0.458/√20 ≈ 0.102
Type-S ≈ Φ(0.842 − 2×0.10/0.102) = Φ(0.842 − 1.96) = Φ(−1.12) ≈ 0.13
```

**Conclusion:** With N=20, there is a 13% chance of detecting drift in the wrong direction. The minimum N for reliable direction detection is approximately 50 per bucket.

### 4.6.2 Type-M Error / Exaggeration Ratio

**Type-M error:** The detected drift magnitude is substantially larger than the true drift.

```
Exaggeration Ratio = E[|estimate| | significant] / |true effect|
```

For an underpowered test (40% power), the exaggeration ratio is approximately 1.8–2.5× — a detected "15pp drift" may actually be a 6–8pp drift that happened to fall on the right side of the significance threshold.

**Protocol implication:** All statistically significant detections should report **confidence intervals**, not just point estimates. The CI is the honest summary; the detected magnitude alone is likely an overestimate at borderline significance.

**Recommended policy:** Do not trigger protocol responses based on a single borderline detection. Require:
- Two consecutive monitoring periods showing the same direction of drift, OR
- A single detection with |z| > 3.0 (well above the threshold), OR
- The system-level ECE increasing by >5pp vs. baseline

---

## 4.7 Complete Protocol Summary (Operational Quick Reference)

```
CALIBRATION MONITORING PROTOCOL v1.0
======================================

SETUP (one-time):
  - Define 5 confidence buckets: [0,0.25), [0.25,0.45), [0.45,0.55),
                                  [0.55,0.75), [0.75,1.0]
  - Establish baseline: collect 200+ resolved outcomes per active bucket
  - Configure EWMA: λ=0.15, L=3.0, warmup=50 per bucket
  - Configure SPRT: α=0.05, β=0.20, δ=0.15 (per bucket)
  - Configure monthly test: α_bucket=0.01 (Bonferroni), two-sided

DAILY:
  - Import newly resolved outcomes into monitoring pool
  - Run EWMA update for each new outcome (Layer 3)
  - Log any Layer 3 alarms with timestamp and EWMA value

WEEKLY (every Monday):
  - Collect past 7 days' resolved outcomes by bucket
  - Run SPRT update for each bucket (Layer 2)
  - Report: SPRT status per bucket, running lambda values
  - Escalate any DRIFT_DETECTED signals per §4.5

MONTHLY (first business day):
  - Run full calibration audit (Layer 1)
  - Generate report using template in §4.4.1
  - Compute ECE, calibration slope, over-all Brier score decomposition
  - Compare ECE to prior month and 3-month rolling average
  - File report; escalate any FLAG signals per §4.5

QUARTERLY:
  - Review all Advisory/Warning/Alarm/Critical events from quarter
  - Assess whether thresholds are appropriate for current system volume
  - Update δ_SPRT if expected drift magnitude has changed
  - Conduct type-M error review: were flagged drifts actually that large?
  - Publish calibration health report for system stakeholders
```
