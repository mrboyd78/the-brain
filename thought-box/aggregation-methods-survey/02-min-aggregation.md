# Deliverable 2: The `min()` Aggregation — When It Is Correct and When It Fails

> **Survey**: Aggregation Methods for Combining Multiple Criterion Scores  
> **Topic**: Exhaustive analysis of weakest-link / minimum aggregation  
> **Date Compiled**: April 2026

---

## 2.1 Introduction and Theoretical Foundations

The `min()` function — selecting the lowest criterion score as the composite — appears deceptively simple. It is, in fact, the canonical aggregation rule for a specific and well-defined class of problems: those where the system fails as soon as any one component fails, and where failures are statistically independent. This is the **series system model** in reliability engineering, formalized by Barlow and Proschan (1975) and by the extreme value statistics tradition (Gumbel, 1958; Galambos, 1978).

Understanding `min()` requires understanding two distinct theoretical roots:

### 2.1.1 Reliability Engineering: Series Systems

**Barlow, R. E., & Proschan, F. (1975).** *Statistical Theory of Reliability and Life Testing: Probability Models.* Holt, Rinehart and Winston.

For a series system of *n* independent components with reliabilities R₁, R₂, ..., Rₙ:

```
R_system = P(all components function)
         = P(min{X₁, X₂, ..., Xₙ} > threshold)
         = ∏ᵢ Rᵢ    (under independence)
```

The failure probability is:
```
F_system = 1 − ∏ᵢ(1 − Fᵢ)    ≈  ∑Fᵢ  (for small individual failure probabilities)
```

When translated to a scoring context (where each score represents the probability that a component exceeds its required performance level), the `min()` of the scores represents the bottleneck: the limiting constraint on system success.

**The chain metaphor**: "A chain is only as strong as its weakest link" is the folk expression of this result. It is *exactly* correct when: (a) the chain breaks if any link breaks, (b) the load on each link is statistically independent.

### 2.1.2 Extreme Value Theory

The mathematical study of `min()` is the study of the **minimum order statistic**, X₍₁₎ = min(X₁, ..., Xₙ). Its distribution is characterized by extreme value theory (EVT):

```
P(X₍₁₎ > t) = P(all Xᵢ > t) = ∏ᵢ P(Xᵢ > t)  (under independence)
```

For i.i.d. random variables, as n → ∞, the minimum order statistic follows a **Weibull distribution** (Type III extreme value), which Weibull (1951) applied directly to material strength — where the material fails at the site of its weakest microscopic flaw.

### 2.1.3 Decision-Theoretic Formulation

In multi-criteria decision analysis (MCDA), `min()` corresponds to the **conjunctive rule** (Dawes, 1979): an alternative is acceptable only if it clears every criterion threshold. The composite score is then the "distance from worst threshold violation":

```
composite = min{(sᵢ − τᵢ) / (sᵢ_max − τᵢ)}  for all i
```

where sᵢ is the criterion score and τᵢ is the threshold. This is the formal basis for certification protocols.

---

## 2.2 Conditions Under Which `min()` Is Correct

The following conditions, taken together, define the regime in which `min()` is the appropriate aggregation rule.

### 2.2.1 Condition 1: True Series (Conjunctive) Structure

**Definition**: The system fails — or the alternative is unacceptable — if and only if any single criterion falls below its threshold. There is no redundancy, no backup, no compensating mechanism.

**Examples where this holds**:
- **Aviation airworthiness certification**: An aircraft must meet structural, avionics, propulsion, and regulatory criteria independently. A perfect propulsion score does not offset a failed structural test.
- **Pharmaceutical safety**: A drug must pass efficacy, toxicology, manufacturing quality, and clinical trial endpoints. Failure on any one = rejection, regardless of others.
- **Contractual compliance**: A bid must satisfy all minimum specifications. No bonus on one item compensates for non-compliance on another (if the contract reads this way).
- **Security clearance**: An applicant must pass background check, psychological evaluation, and polygraph. Any failure = rejection.

**Formal test**: Write the acceptance rule explicitly. If it reads "all of the following must be true", the structure is conjunctive → `min()` is appropriate.

### 2.2.2 Condition 2: Statistically Independent Failure Modes

**Definition**: P(criterion i fails | criterion j has failed) = P(criterion i fails). The failure events are independent.

**Examples where this holds**:
- Physical systems where components fail through distinct, unrelated mechanisms (corrosion of one pipe vs. fatigue of a bearing)
- Multiple-evaluator panels where each evaluator independently scores a different dimension of a submission
- Multi-parameter laboratory tests where each measurement is made on a separate instrument

**Why independence matters for `min()`**: Under independence, P(min < threshold) = 1 − ∏ P(Xᵢ ≥ threshold). Under positive dependence, this formula *underestimates* the joint probability of all criteria being above threshold because co-failures are more likely than independence implies. The `min()` in scoring (as opposed to probability) is not directly affected by this formula, but the *interpretation* changes: when criteria are correlated, the `min()` composite is driven by a single latent factor, not by a genuine bottleneck.

### 2.2.3 Condition 3: Non-Compensable Criteria

**Definition**: No amount of surplus on criterion j can offset a deficit on criterion i.

**Examples**:
- A food product fails a contamination test (criterion i = 0). It cannot be "saved" by excellent nutritional labeling (criterion j = 1.0).
- A safety valve fails its pressure rating. It cannot be approved because the facility has excellent fire suppression.
- A job candidate lacks a required credential. They cannot be hired because they have exceptional experience in other areas (if the credential is a legal requirement).

**Formal test**: Ask the decision-maker: "What score on criterion j would make you indifferent between this alternative and a perfect alternative that fails criterion i?" If the answer is "no score is sufficient", → non-compensable.

### 2.2.4 Condition 4: Low Measurement Noise Relative to Score Variability

**Definition**: The measurement error σᵢ on each criterion is small relative to the true spread of scores across alternatives (σᵢ << σ_true_i).

**Why this matters**: `min()` selects the *realized* minimum, which is the true minimum plus measurement error on whichever dimension happens to be lowest. If measurement noise is substantial, the realized minimum may be a noise artifact: an alternative with a "true" minimum of 0.70 may observe a realized minimum of 0.55 due to measurement error, being mis-ranked against an alternative whose true minimum is 0.60 but whose realized minimum is 0.61.

**Threshold guidance**: If σᵢ / (max_score − min_score) > 0.10–0.15 on any criterion, noise will materially distort `min()` aggregation. (See Deliverable 4 for quantified analysis.)

### 2.2.5 Condition 5: The Score Scale Captures the True Bottleneck

**Definition**: The lowest-scored criterion is actually the binding constraint on system performance, not merely the hardest to score highly on due to scale construction or normalization artifacts.

**Pitfall**: If criterion i is scored on a 0–10 scale where 5 is "adequate" and criterion j is scored on a 0–10 scale where 7 is "adequate", then `min()` in raw score space does not correctly identify the binding constraint. The normalization must be done relative to the threshold or the distribution of scores before applying `min()`.

---

## 2.3 Conditions Under Which `min()` Is Wrong (or Misleading)

### 2.3.1 Failure Condition 1: Positively Correlated Criterion Scores

**What goes wrong**:

When criteria are driven by a common latent factor (e.g., same rater, same testing environment, same underlying capability), then:

- A low score on criterion i tends to co-occur with low scores on other criteria
- The `min()` becomes essentially a measure of the latent factor, but with *double-penalization*: the alternative is punished for having the common factor low (which already dragged all criteria down) and then punished again for the fact that the lowest criterion is the minimum of several already-correlated low scores

**Quantitative illustration**:

Suppose criteria i and j have true scores both = 0.70, correlated at ρ = 0.90, with noise σ = 0.10.
- Expected min(true) ≈ 0.70 − 0.10·k(2) ≈ 0.64 for independent criteria
- Under ρ = 0.90, E[min(X,Y)] ≈ 0.70 − 0.10·k(2,ρ) ≈ 0.67 — higher than the independent case
- But the realized extreme is now driven by the shared latent factor, not by a genuine independent bottleneck

**Practical implication**: In correlated multi-disciplinary evaluation panels (where halo effects are common), `min()` across scores from the same rater double-counts that rater's overall impression. Correct handling: use partial correlations or factor-adjust before taking `min()`.

**Reference**: Barlow & Proschan (1975, Ch. 3) explicitly derive that common-cause failures (positively correlated components) cause the series system reliability to *exceed* the independence-based product formula — the correct formula requires copula modeling (Joe, 1997).

### 2.3.2 Failure Condition 2: Compensable Criterion Structure

**What goes wrong**:

When trade-offs are genuinely acceptable — when the decision-maker *would* accept a slightly weaker performance on criterion i in exchange for substantially stronger performance on criterion j — then `min()` fails by ignoring that preference.

**Example**: A hiring committee evaluating candidates on technical skill (0.80), communication (0.55), and leadership (0.90). If communication and technical skill are explicitly compensable (the job description says "strong technical skills or strong communication, with a minimum of the other"), then `min()` = 0.55 for this candidate, which may under-rank them versus a less technically skilled candidate with uniform 0.65 scores.

**Formal diagnosis**: `min()` implies that the marginal rate of substitution between any two criteria is zero — that is, no amount of performance on criterion j has any value once it exceeds the minimum. This is a strong and usually wrong assumption for compensable criteria.

### 2.3.3 Failure Condition 3: High Measurement Noise on the Lowest Criterion

**What goes wrong**:

`min()` concentrates all measurement noise in a single observed value — the realized minimum. Even if a particular criterion is not the *true* worst performer, if its measurement error happens to produce the lowest observed score, it determines the entire composite.

**Mathematical formulation**:

Let X̂ᵢ = Xᵢ + εᵢ where εᵢ ~ N(0, σ²) (independent measurement errors).

```
composite_min = min{X̂₁, ..., X̂ₙ}
             = min{X₁+ε₁, ..., Xₙ+εₙ}
```

Even if X₁ = X₂ = ... = Xₙ = μ (all criteria equal), the composite under `min()` has:

```
E[composite_min] = μ − σ · E[max{Z₁,...,Zₙ}]  where Zᵢ ~ N(0,1)
                 ≈ μ − σ · √(2 ln n)  (asymptotically for large n)
```

For n = 6 criteria, √(2 ln 6) ≈ 1.90, so the expected composite lies 1.90σ *below* the true mean. This bias grows with n. The arithmetic mean, by contrast, has:

```
E[composite_AM] = μ  (unbiased regardless of n)
```

**Implication**: `min()` is systematically biased downward in noisy environments, with the bias growing as more criteria are added. An alternative evaluated on 10 criteria will be systematically disadvantaged relative to one evaluated on 3 criteria, purely from noise concentration.

### 2.3.4 Failure Condition 4: Parallel (Redundant) System Structure

**What goes wrong**:

A parallel system — where the system succeeds if *any* component succeeds — should use `max()`, not `min()`. Using `min()` on a parallel system is the mirror-image error: it treats a system with genuine redundancy as if it had no backup, producing an artificially pessimistic composite.

**Example**: A data center with three independent power supplies (any one is sufficient) is a parallel system. The reliability is 1 − ∏(1−Rᵢ) ≈ 1 for high-reliability components. Applying `min()` to the three power supply reliability scores would give the lowest, which understates the actual system reliability.

### 2.3.5 Failure Condition 5: Criteria with Different Scales or Thresholds (Not Normalized)

**What goes wrong**:

Raw `min()` without normalization identifies the criterion with the lowest absolute value, which may not be the criterion closest to its threshold or the actual binding constraint.

**Example**: Criterion A scores 4.0/10 (threshold = 3.0; 33% above threshold). Criterion B scores 6.5/10 (threshold = 6.0; 8% above threshold). Raw `min()` = 4.0 (criterion A), but criterion B is actually the binding constraint in terms of margin above the threshold.

**Fix**: Normalize to margin above threshold before applying `min()`:
- Criterion A normalized: (4.0 − 3.0) / (10 − 3.0) = 0.143
- Criterion B normalized: (6.5 − 6.0) / (10 − 6.0) = 0.125
- Normalized `min()` = 0.125 → correctly identifies criterion B as the binding constraint.

---

## 2.4 Weighted-Min as a Partial Remedy

The weighted `min()` applies differential weights to each criterion before taking the minimum:

```
composite_wmin = min{w₁·s₁/w_max, w₂·s₂/w_max, ..., wₙ·sₙ/w_max}
```

or equivalently identifies the minimum of the weight-normalized scores:

```
composite_wmin = min{sᵢ/wᵢ}  (treating weights as thresholds)
```

**When weighted-min helps**:
- When some criteria are more critical than others but all are non-compensable: weights allow the less-critical criteria to have lower effective thresholds
- When measurement noise is known to be higher on some criteria: down-weighting noisier criteria reduces the bias from noise concentration

**When weighted-min still fails**:
- It does not address correlated failures — the correlation problem persists regardless of weights
- It does not address the compensability issue — it remains a purely conjunctive rule
- The choice of weights is arbitrary in the absence of a formal model for relative criticality

---

## 2.5 Adjacent Methods and Their Relationship to `min()`

| Method | Relationship to min() | When to Use Instead |
|---|---|---|
| Fuzzy minimum | Generalizes hard min() to soft floors; allows partial failure | When failure is graded rather than binary |
| OWA with orness=0 | Mathematically equivalent to min() | When you want the OWA family's consistency |
| Conjunctive threshold + AM | Hard gates on critical criteria; AM on remainder | When some criteria are non-compensable and others are not |
| Lexicographic | Min() on most critical criterion first; then second, etc. | When there is a priority order among criteria |
| Copula-based reliability | Generalizes product formula to dependent failures | When failure modes are correlated |

---

## 2.6 Practical Checklist: Applying min() Safely

Before applying `min()`, verify each of the following:

- [ ] **Conjunctive rule confirmed**: The system/alternative genuinely fails if any one criterion fails
- [ ] **Independence verified or accounted for**: Criteria do not share a common cause; or copula modeling has been used if they do
- [ ] **Non-compensability confirmed**: Decision-maker cannot specify any trade-off rate between failing and passing criteria
- [ ] **Scale normalization done**: All criteria normalized relative to their threshold before taking min()
- [ ] **Noise assessed**: σᵢ / range < 0.10 on all criteria; or weights adjusted to reflect measurement reliability
- [ ] **n is small-to-moderate**: With many criteria (n > 8), the minimum order statistic's downward bias from noise becomes very large; consider robust min (e.g., second-order statistic) if n is large

---

## 2.7 Key Citations

- Barlow, R. E., & Proschan, F. (1975). *Statistical Theory of Reliability and Life Testing: Probability Models.* Holt, Rinehart & Winston.
- Dawes, R. M. (1979). The robust beauty of improper linear models in decision making. *American Psychologist*, 34(7), 571–582.
- Galambos, J. (1978). *The Asymptotic Theory of Extreme Order Statistics.* Wiley.
- Gumbel, E. J. (1958). *Statistics of Extremes.* Columbia University Press.
- Joe, H. (1997). *Multivariate Models and Dependence Concepts.* Chapman and Hall.
- Keeney, R. L., & Raiffa, H. (1976). *Decisions with Multiple Objectives.* Wiley.
- Munda, G. (2005). Multiple criteria decision analysis and sustainable development. In J. Figueira et al. (Eds.), *Multiple Criteria Decision Analysis: State of the Art Surveys.* Springer.
- OECD/JRC. (2008). *Handbook on Constructing Composite Indicators.* OECD Publishing.
- Weibull, W. (1951). A statistical distribution function of wide applicability. *Journal of Applied Mechanics*, 18(3), 293–297.
- Yager, R. R. (1988). On ordered weighted averaging aggregation operators. *IEEE Transactions on Systems, Man, and Cybernetics*, 18(1), 183–190.
