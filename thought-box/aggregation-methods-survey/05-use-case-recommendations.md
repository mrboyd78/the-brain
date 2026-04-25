# Deliverable 5: Recommended Aggregation for Three Generic Use Cases

> **Survey**: Aggregation Methods for Combining Multiple Criterion Scores  
> **Topic**: Safety certification, decision quality scoring, ordinal preference ranking  
> **Date Compiled**: April 2026

---

## 5.1 Overview

This deliverable provides a full, evidence-backed recommendation for three representative use cases that cover the major decision structures encountered in practice. Each use case is treated exhaustively: the structural properties of the problem are identified, the set of appropriate methods is derived, the primary method is recommended with full justification, and failure modes of alternative methods are documented.

The three use cases are:
1. **Safety certification** — non-compensable axes, independent failures, binary pass/fail outcome
2. **Decision quality scoring** — noisy cardinal axes, partially compensable, comparative ranking
3. **Preference ranking with ordinal inputs** — no cardinal scores, only rank orders available, social choice problem

---

## 5.2 Use Case A: Safety Certification with Non-Compensable Axes

### 5.2.1 Problem Characterization

**Domain examples**: Aviation airworthiness (FAA/EASA certification), nuclear plant licensing (NRC), pharmaceutical approval (FDA/EMA), food safety compliance, cybersecurity certification (ISO 27001 audits), structural engineering code compliance.

**Decision type**: Binary pass/fail determination across a set of mandatory criteria. Each criterion represents a distinct safety-relevant requirement. Failing any criterion = overall failure, regardless of performance on others.

**Criterion properties**:
- **Compensability**: None — by regulatory and ethical design, no criterion can be traded off against another
- **Failure mode structure**: Typically independent (distinct physical or procedural systems)
- **Scale**: Often binary (pass/fail per sub-criterion) or ratio-scale (0 to 1, where any score below threshold τᵢ = failure)
- **Noise**: Low-to-moderate (rigorous testing procedures); but critical criteria often tested multiple times

### 5.2.2 Recommended Method: Conjunctive Threshold Rule (min() variant)

**Mathematically**:
```
Decision = PASS if and only if ŝᵢ ≥ τᵢ for all i
         = FAIL if any ŝᵢ < τᵢ
```

In composite scoring form:
```
composite = min{(ŝᵢ − τᵢ) / (sᵢ_max − τᵢ)}   for i = 1,...,n
```

This normalizes each criterion score to its margin above threshold: a composite of 0.0 means exactly at threshold; 1.0 means at the maximum possible score; negative values mean a threshold violation.

**Theoretical foundation**: Barlow & Proschan (1975) series system model. Under independent failure modes, the probability that a system passes all criteria simultaneously is:

```
P(pass all) = ∏ᵢ P(ŝᵢ ≥ τᵢ)
```

The conjunctive rule is the only aggregation method consistent with this probability model when failure modes are independent.

**Regulatory practice**:

The conjunctive structure is explicitly mandated in:
- **FAA Advisory Circular AC 25.1309** (aircraft airworthiness): systems analysis must demonstrate compliance of every system individually; no cross-system compensation is permitted
- **FDA 21 CFR Part 820** (quality system regulation for medical devices): each quality system element must independently meet standards
- **IEC 61511 (functional safety)**: Safety Integrity Levels (SILs) are assigned per safety function; no averaging across functions
- **NFPA 101 (Life Safety Code)**: each egress, sprinkler, and detection requirement is individually mandatory

### 5.2.3 Implementation Details

**Step 1: Threshold normalization**

Before applying `min()`, normalize all criteria to their margin above threshold:
```
normalized_score_i = (ŝᵢ − τᵢ) / (max_score_i − τᵢ)
```

Positive = above threshold; zero = exactly at threshold; negative = failure.

**Step 2: Apply min() to normalized scores**

The composite is the normalized margin of the most-critical (most-deficient) criterion.

**Step 3: Soft fail vs. hard fail**

For regulatory purposes, any negative composite triggers a hard fail. For internal quality improvement, the composite value (even if positive) identifies the bottleneck criterion for investment: the criterion with the lowest normalized margin is the weakest link.

**Step 4: For correlated failures — use fault-tree analysis**

If common-cause failures are possible (e.g., shared power supply, same environmental stress), the product formula for P(pass all) underestimates the joint failure probability. In this case, use:
- Common-cause failure (CCF) models (IEC 61508, Appendix D)
- Beta-factor model: P(CCF) = β · (sum of individual failure rates)
- Fault-tree AND/OR gate propagation with correlation adjustments

**Step 5: Uncertainty handling on threshold margins**

When measurement uncertainty σᵢ is significant on a safety criterion:
- Require the lower bound of the 95% confidence interval to clear the threshold: ŝᵢ − 1.645·σᵢ ≥ τᵢ
- This is consistent with the "demonstrated margin" approach in MIL-HDBK-217F (reliability prediction of electronic systems)

### 5.2.4 Methods to Avoid in This Use Case

| Method | Why It Fails |
|---|---|
| Arithmetic mean | Allows criterion failures to be averaged away; a score of 0 on C1 can produce a passing composite if others are high |
| Geometric mean | Partially penalizes zero scores but does not guarantee rejection; zero × anything = zero only in pure GM without floor |
| Weighted mean | Same failure as AM; the weights govern the impact of a failure but do not prohibit compensation |
| Borda count | Operates on ranks, not thresholds; a rank-first on all other criteria cannot compensate for threshold violation |
| Simple mean without normalization | Applies uniform aggregation without accounting for different threshold positions; produces spurious orderings |

### 5.2.5 Adjacent Method: Veto Thresholds in MAUT

When a decision problem is mostly compensable (a grant evaluation, a vendor selection) but has one or two mandatory constraints, the appropriate hybrid method is:

1. Apply hard veto: eliminate any alternative with ŝᵢ < τᵢ on non-compensable criteria
2. Apply weighted arithmetic mean to the remaining (compensable) criteria for the surviving alternatives
3. Rank survivors by their compensable composite score

This two-stage approach (used in practice in EU tender evaluations and grant panels) correctly separates the conjunctive logic from the compensatory logic.

---

## 5.3 Use Case B: Decision Quality Scoring with Noisy Axes

### 5.3.1 Problem Characterization

**Domain examples**: Research grant evaluation, job candidate assessment, performance management scoring, investment opportunity ranking, software quality assessment, AI model benchmarking.

**Decision type**: Comparative ranking or scoring of alternatives across multiple quality dimensions. The goal is to identify the best or best few alternatives, not a binary pass/fail.

**Criterion properties**:
- **Compensability**: Partial to full — being weaker on one dimension can be offset by being stronger on another, subject to domain-specific limits
- **Failure mode structure**: Not truly independent — same evaluator may score all criteria (halo effect); underlying quality construct may be correlated across criteria
- **Scale**: Cardinal 0–1 or standardized interval scales; but measurement error is substantial (expert judgment has σ ≈ 0.05–0.15 per unit range in practice)
- **Noise**: Moderate to high; multiple raters per criterion reduce noise but introduce inter-rater reliability issues

### 5.3.2 Recommended Method: Weighted Arithmetic Mean with Reliability-Based Weights

**Primary recommendation**: Weighted arithmetic mean (additive MAUT under utility independence)

**Formula**:
```
composite(x) = Σᵢ wᵢ · sᵢ    where Σwᵢ = 1
```

**Justification**:

(a) **Noise cancellation**: As derived in Deliverable 4, σ_WAM = σ · √(Σwᵢ²), which for reasonable weight distributions is substantially less than σ_individual. Under i.i.d. criterion errors, the composite error is ~30–50% of individual criterion error.

(b) **Compensability implementation**: The additive form is the unique decomposition of a utility function when attributes are mutually utility-independent, preferential independence holds, and trade-off rates are constant (Keeney & Raiffa, 1976, Theorem 5.1). Both conditions are typically satisfied in quality-scoring problems.

(c) **Interpretability**: Weighted average is easily explained to stakeholders and auditable — each criterion's contribution is wᵢ × sᵢ.

(d) **Robustness**: Dawes (1979) showed that even random weights (as long as they are in the correct direction) produce near-optimal decisions in linear additive models. The AM is the most forgiving method to weight specification errors.

### 5.3.3 Weight Elicitation Methodology

Weights should reflect the relative importance of criteria, not their variance. Three approaches:
- **Swing weights** (Keeney & Raiffa, 1976): Decision-maker states the most preferred and least preferred score on each criterion; identifies how much they would "swing" to move from least to most preferred; weights are proportional to these swings
- **Pairwise ratio weights** (AHP, Saaty, 1980): Decision-maker compares each pair of criteria on a 1–9 scale of relative importance; weights derived by eigenvector method
- **Rank-order weights with ROC approximation**: If exact weights are unavailable, rank criteria by importance and approximate: wᵢ ≈ (1/rank_i) / Σ(1/rank_j)

**Warning on equal weights**: Equal weights are appropriate only when all criteria are genuinely equally important. In practice, equal weights are often used for convenience and can produce rankings inconsistent with stated preferences. If the decision-maker cannot discriminate between criterion importances, Borda count (ordinal) may be more defensible.

### 5.3.4 Handling Correlated Criterion Errors (Same Rater)

When the same evaluator scores all criteria for all alternatives, inter-criterion errors become correlated (halo effect, ρ ≈ 0.30–0.60 typically). This inflates the composite noise (as shown in Deliverable 4):

```
σ_WAM_correlated = σ_WAM_independent · √[(1 + (n−1)ρ_avg)]
```

For n = 6, ρ = 0.50: σ_WAM_correlated ≈ 1.87 × σ_WAM_independent

**Mitigation strategies**:
1. **Multiple independent raters**: Assign different raters to different criteria (reduces ρ toward 0)
2. **Blind scoring**: Raters do not see other criteria scores while scoring (reduces anchoring-driven correlation)
3. **Factor adjustment**: Regress out evaluator-level mean from each score before aggregating
4. **Intraclass correlation correction**: Shrink composite scores toward the group mean by a factor of (1 − ρ̂_rater) following Shrout & Fleiss (1979)

### 5.3.5 When to Switch to Geometric Mean

If the decision-maker's preferences show **decreasing marginal value for performance on any single criterion** — i.e., being excellent on one criterion is worth less when another criterion is very weak — the geometric mean is appropriate:

```
composite_GM(x) = ∏ᵢ sᵢ^wᵢ
```

The GM imposes a **concave trade-off structure**: it penalizes imbalance. An alternative that scores [0.95, 0.40] receives GM = 0.95^0.5 × 0.40^0.5 = 0.617, vs. AM = 0.675. The GM preference is for alternatives that score [0.675, 0.675] over [0.95, 0.40].

**Test for GM suitability**: Ask the decision-maker: "Would you prefer a candidate who scores 0.90 on all criteria over one who scores 1.0 on five and 0.30 on one?" If yes → GM is more consistent with preferences than AM.

**GM limitations** (see Deliverable 4): GM amplifies noise near zero. Institute a floor (e.g., replace any sᵢ < 0.05 with 0.05) before computing GM.

### 5.3.6 Methods to Avoid in This Use Case

| Method | Why It Fails |
|---|---|
| `min()` | Concentrates all noise in the worst-performing criterion; poor rank stability (68% correct #1 vs. AM's 82%); ignores genuine compensability |
| Borda count | Treats ordinal gaps equally; a candidate barely ahead on 5 criteria beats one far ahead on 4 and behind on 1 — inappropriate for cardinal data |
| Unweighted AM when weights differ | If C1 is 4× more important than C6, unweighted AM distorts the ranking in the direction of unimportant criteria |

---

## 5.4 Use Case C: Preference Ranking with Ordinal Inputs

### 5.4.1 Problem Characterization

**Domain examples**: Election/voting design, grant or prize selection committees where raters provide ranks not scores, consumer preference surveys with Likert-type responses, selection from candidates using evaluators who cannot provide calibrated cardinal judgments.

**Decision type**: Produce a group ranking or winner selection from individual rank orders.

**Criterion properties**:
- **Scale**: Ordinal only — rank orders or ordinal categories (strongly agree, agree, neutral, disagree, strongly disagree) where the intervals between categories are not necessarily equal
- **Cardinal arithmetic is not justified**: Computing means of Likert ratings (e.g., 1–5 scale as if interval) is common but formally incorrect
- **Compensability**: Undefined at the cardinal level; depends on the voting rule applied

### 5.4.2 Recommended Method for Winner Selection: Borda Count with Domain Restriction Check

**Primary recommendation**: **Borda count** for ranked-choice inputs where a winner selection is needed; **pairwise Condorcet with Schulze/Ranked Pairs tiebreaking** for a full ranking.

**Formula (Borda)**:
```
Borda_score(alternative a) = Σᵢ (rank_i(a) reversed and scaled)
                           = Σᵢ (n − rank_i(a))  for n alternatives
```

**When Borda is appropriate**:
- The alternatives differ in rank order across criteria but not necessarily in magnitude
- Cardinal information is unavailable or unreliable
- The group wants a transparent, summable rule that can be computed without utility elicitation

**Theoretical foundation**: 
- **May (1952)**: Majority rule is the unique rule satisfying anonymity, neutrality, positive responsiveness, and decisiveness — for two alternatives only. Borda generalizes naturally to more alternatives.
- **Young (1988)**: The Borda count is the maximum likelihood estimator of the true ordering when true preferences are generated by a Mallows (distance-based) model — i.e., it finds the ordering closest (in sum of Kendall-tau distances) to all individual rankings.

### 5.4.3 Arrow's Impossibility Theorem and What It Means Practically

**Arrow (1951)**: No social welfare function with ≥3 alternatives can simultaneously satisfy:
1. **Unrestricted Domain (U)**: Works for any preference profile
2. **Pareto Efficiency (P)**: If everyone prefers A to B, the group prefers A to B
3. **Independence of Irrelevant Alternatives (IIA)**: Ranking of A vs. B depends only on individual A vs. B rankings, not on C
4. **Non-Dictatorship (D)**: No single individual determines the group ranking

**Which axiom does each method violate?**:

| Method | Violated Axiom | Consequence |
|---|---|---|
| Majority rule | U (cycles) | May produce A > B > C > A (Condorcet paradox) |
| Borda count | IIA | Adding/removing an alternative can reverse the A vs. B ranking |
| Approval voting | IIA | Adding an alternative changes approval thresholds |
| Any dictatorship | D | One person decides, ignores others |

**Practical implication**: Arrow's theorem is a theorem about *impossibility*, not about paralysis. In practice:
- Use **Borda** when IIA violations are acceptable (stable alternative set; no strategic manipulation expected)
- Use **Condorcet** when cycles are unlikely (single-peaked preferences; ideologically organized choices)
- Use **Approval voting** when the threshold is more natural than the full ranking (voters know who is acceptable, not by how much)

### 5.4.4 Condorcet Jury Theorem and Epistemic Voting

**When the goal is to find the objectively correct answer (not just aggregate preferences)**:

**Condorcet Jury Theorem** (classical): If each voter independently has probability p > 0.5 of voting for the correct alternative, majority rule converges to the correct answer as group size grows.

**List & Goodin (2001)**: Generalized CJT to:
- Plurality voting with k > 2 alternatives: correct choice wins if each voter's probability of voting correctly > 1/k
- Variable competence: group still converges if *average* competence > 0.5

**Dietrich (2008)**: Critical limitation — the competence and independence assumptions are jointly unjustifiable in most real settings. Specifically:
- If voters share a common information set, their errors are correlated → the group does not converge as fast as CJT predicts
- If voters are competent *because* they reason from the same information, the independence assumption fails

**Practical recommendation from the Dietrich critique**:
- For epistemic aggregation (finding the correct answer), the Condorcet/majority approach is valid only when voters have *genuinely independent* information sources
- If all jurors were shown the same evidence by the same prosecution, the CJT's independence premise fails
- Solution: diversify information sources across deliberators before aggregation (consistent with Page's Diversity Prediction Theorem)

### 5.4.5 Approval Voting

**When to use**: When the group needs a "threshold-based" selection and cannot reliably rank all alternatives, but can identify which alternatives are acceptable.

**Formula**:
```
Approval_score(a) = number of voters who approve alternative a
```

**Properties** (Brams & Fishburn, 1983):
- Satisfies Pareto efficiency and non-dictatorship
- Violates IIA (like Borda)
- Produces less strategic manipulation than plurality voting
- Natural in panel selection: "approve all candidates you find acceptable for the position"

**Limitation**: The approval threshold is subjective and varies across voters, which can introduce inconsistency. Approval voting does not produce a full ranking — only the top alternative(s).

### 5.4.6 Handling the Condorcet Paradox

When pairwise majority comparisons produce cycles (A > B, B > C, C > A), one of three strategies applies:

| Strategy | Method | Appropriate When |
|---|---|---|
| **Tiebreaking** | Schulze method (beat-path) or Ranked Pairs | A full ordering is needed; Condorcet winner doesn't exist |
| **Restriction** | Restrict to single-peaked domain; use median voter | Preferences lie on one dimension (political spectrum) |
| **Randomization** | Maximal lottery; randomized social choice | Fairness requires stochastic outcome; no deterministic winner is justified |

**Single-peaked preferences theorem** (Black, 1948): If all individual preference orders are single-peaked on a common dimension (e.g., political left-right), majority preferences are transitive and the median voter's preference is the majority winner. This is the key sufficient condition under which Condorcet paradoxes cannot occur.

### 5.4.7 Page's Diversity Prediction Theorem (for Numerical Aggregation of Estimates)

**When the problem is epistemic** (estimating a true value, not just choosing a preferred one), and when individual estimates are numerical (not just ranks), Page's Diversity Prediction Theorem (2007, 2017) provides the correct aggregation:

```
Crowd Error = Average Individual Error − Prediction Diversity
```

This is a mathematical identity. Its practical implication for aggregation design:
- **Do not just use the best individual's estimate** — unless Prediction Diversity ≥ Average Individual Error (which would mean the crowd is anti-diversified), the crowd mean always outperforms the average individual
- **Maximize predictive diversity** by selecting estimators with cognitively distinct models, heuristics, and information sources
- **The arithmetic mean is the correct aggregation** under the DPT when estimators are unbiased and independent — the DPT justifies averaging as epistemically optimal

**DPT relationship to weighted aggregation**: If some estimators are known to be more accurate than others, weights proportional to (1/estimated_error²) are optimal (analogous to inverse-variance weighting in meta-analysis). This is the correct use of expertise differences — not to give expert estimates more weight in a non-epistemic vote, but to weight by demonstrated accuracy.

---

## 5.5 Summary Recommendation Table

| Use Case | Problem Structure | Recommended Primary Method | Secondary Method | Methods to Avoid |
|---|---|---|---|---|
| **Safety certification** | Non-compensable, independent failures, binary pass/fail | Conjunctive threshold rule (`min()` on normalized margins) | Fault-tree / bow-tie for correlated failures | AM, GM, Borda |
| **Decision quality scoring** | Compensable axes, noisy cardinal scores, comparative ranking | Weighted arithmetic mean (MAUT additive form) | Geometric mean (if imbalance-penalty needed) | `min()`, unweighted Borda |
| **Ordinal preference ranking** | Rank inputs only, no cardinal scores, social choice | Borda count (winner); Condorcet/Schulze (full ranking) | Approval voting (if threshold-based) | AM of ordinal ranks |

---

## 5.6 Failure-Mode Map: Common Mismatches

| Problem Structure | Mistakenly Used Method | Symptom of Error | Correct Fix |
|---|---|---|---|
| Non-compensable, certification | AM | Failing alternatives receive passing composites | Replace with conjunctive threshold |
| Noisy axes, compensable | `min()` | Rank instability; rankings change with new evaluation cycle | Replace with weighted AM |
| Ordinal inputs | AM (mean of ranks) | Cardinal arithmetic unjustified; IIA violated implicitly | Use Borda or Condorcet |
| Correlated failure modes | Independent `min()` / series product | Under-estimates joint failure probability | Use copula/CCF model; fault-tree |
| Heterogeneous sub-populations | AM across groups | Simpson's paradox; aggregate reverses within-group ordering | Stratify before aggregating |
| Many criteria (n > 8) with noise | `min()` | Systematic downward bias grows with n; rank distortion | Switch to AM; use reliability weights |

---

## 5.7 Key Citations

- Arrow, K. J. (1951). *Social Choice and Individual Values.* Wiley.
- Barlow, R. E., & Proschan, F. (1975). *Statistical Theory of Reliability and Life Testing.* Holt, Rinehart & Winston.
- Black, D. (1948). On the rationale of group decision-making. *Journal of Political Economy*, 56(1), 23–34.
- Brams, S. J., & Fishburn, P. C. (1983). *Approval Voting.* Birkhäuser.
- Dawes, R. M. (1979). The robust beauty of improper linear models in decision making. *American Psychologist*, 34(7), 571–582.
- Dietrich, F. (2008). The premises of Condorcet's jury theorem are not simultaneously justified. *Episteme*, 5(1), 56–73.
- FAA. (2000). *Advisory Circular AC 25.1309-1A: System Design and Analysis.* Federal Aviation Administration.
- IEC 61508. (2010). *Functional Safety of Electrical/Electronic/Programmable Electronic Safety-Related Systems.* International Electrotechnical Commission.
- IEC 61511. (2016). *Functional Safety: Safety Instrumented Systems for the Process Industry.* IEC.
- Keeney, R. L., & Raiffa, H. (1976). *Decisions with Multiple Objectives.* Wiley.
- List, C., & Goodin, R. E. (2001). Epistemic democracy: Generalizing the Condorcet jury theorem. *Journal of Political Philosophy*, 9(3), 277–306.
- May, K. O. (1952). A set of independent necessary and sufficient conditions for simple majority decision. *Econometrica*, 20(4), 680–684.
- OECD/JRC. (2008). *Handbook on Constructing Composite Indicators.* OECD Publishing.
- Page, S. E. (2007). *The Difference: How the Power of Diversity Creates Better Groups, Firms, Schools, and Societies.* Princeton University Press.
- Page, S. E. (2017). *The Diversity Bonus.* Princeton University Press.
- Saaty, T. L. (1980). *The Analytic Hierarchy Process.* McGraw-Hill.
- Shrout, P. E., & Fleiss, J. L. (1979). Intraclass correlations: Uses in assessing rater reliability. *Psychological Bulletin*, 86(2), 420–428.
- Young, H. P. (1988). Condorcet's theory of voting. *American Political Science Review*, 82(4), 1231–1244.
- Young, H. P. (1995). Optimal voting rules. *Journal of Economic Perspectives*, 9(1), 51–64.
