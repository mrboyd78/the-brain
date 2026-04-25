# Deliverable 1: Canonical VoI Formula Derivation — EVPI, EVSI, EVII

> **Survey**: Formal Theory of Value of Information  
> **Topic**: Complete derivations with worked examples  
> **Sources**: Howard (1966); Raiffa (1968); Savage (1954); Clemen & Reilly (2001)  
> **Date Compiled**: April 2026

---

## 1.1 Foundational Setup: Decision-Theoretic Framework

### 1.1.1 The Basic Decision Problem

The value of information emerges from the structure of a decision problem under uncertainty. Define:

- **Θ** = the state space (the uncertain quantity the decision-maker cares about); θ ∈ Θ is a specific state
- **A** = the action space; a ∈ A is a specific action/decision
- **u(a, θ)** = the utility function mapping actions and states to utility values
- **p(θ)** = the decision-maker's prior probability distribution over states (their current beliefs)

The **optimal action without information** is:

```
a* = argmax_{a ∈ A} E_θ[u(a, θ)] = argmax_{a ∈ A} ∫ u(a, θ) p(θ) dθ
```

The **expected utility without information** (the baseline) is:

```
EU* ≡ max_{a ∈ A} ∫ u(a, θ) p(θ) dθ
```

**Howard's key insight (1966)**: Information has value only if it *changes the optimal action*. If the same action a* is optimal for all possible information signals, the information is worthless no matter how much uncertainty it resolves.

### 1.1.2 Savage's Subjective Expected Utility Foundation

Savage (1954) proved that a decision-maker who satisfies his seven axioms of rational choice behaves as if maximizing expected utility with a subjective probability distribution. This foundational result means that the value of information can be measured entirely in utility units — there is no need to separate the "information content" from the "decision consequences."

The VoI framework is therefore grounded in:
1. **Subjective Bayesianism**: probabilities are the decision-maker's beliefs, not objective frequencies
2. **Expected utility maximization**: the criterion against which information value is measured
3. **Coherence**: the decision-maker's probabilities and utilities are jointly consistent

---

## 1.2 Expected Value of Perfect Information (EVPI)

### 1.2.1 Formal Derivation

Suppose a clairvoyant reveals the true state θ before the decision is made. For each revealed θ, the rational decision-maker chooses:

```
a*(θ) = argmax_{a ∈ A} u(a, θ)
```

The **expected utility with perfect information** is:

```
EU_{PI} = E_θ[max_{a ∈ A} u(a, θ)] = ∫ [max_{a ∈ A} u(a, θ)] p(θ) dθ
```

**Definition**: The Expected Value of Perfect Information is:

```
EVPI ≡ EU_{PI} − EU*
      = E_θ[max_{a ∈ A} u(a, θ)] − max_{a ∈ A} E_θ[u(a, θ)]
```

**Proof that EVPI ≥ 0**: By Jensen's inequality applied to the max operator:

```
E_θ[max_{a} u(a, θ)] ≥ max_{a} E_θ[u(a, θ)]
```

This holds because the max of expectations is at most the expectation of the max. Perfect information allows the decision-maker to optimize for each realized θ; without it, they must optimize for the average. ∎

**Proof that EVPI = 0 iff the same action is optimal for all states**:

If ∃ a' such that u(a', θ) ≥ u(a, θ) for all θ ∈ Θ (a dominates all other actions in every state), then:

```
max_{a} u(a, θ) = u(a', θ)  for all θ
EU_{PI} = ∫ u(a', θ) p(θ) dθ = EU*
EVPI = 0
```

Conversely, if no action dominates, there exist states where different actions are optimal, so knowing θ changes the choice, and EVPI > 0. ∎

### 1.2.2 EVPI in the Finite State Case

For a discrete state space Θ = {θ₁, ..., θₙ} with prior probabilities {p₁, ..., pₙ}:

```
EU_{PI} = Σᵢ pᵢ · max_{a ∈ A} u(a, θᵢ)
EU*     = max_{a ∈ A} Σᵢ pᵢ · u(a, θᵢ)
EVPI    = EU_{PI} − EU*
```

### 1.2.3 Worked Example: Oil Exploration Decision

**Setup** (adapted from Raiffa, 1968):

A petroleum company must decide whether to drill (D) or not drill (ND) on a tract. The uncertain quantity is the geological structure: high oil (H) or dry (L).

| Action | State: High (θ_H) | State: Dry (θ_L) |
|---|---|---|
| Drill (D) | +$15M | −$8M |
| Not Drill (ND) | $0 | $0 |

Prior belief: P(θ_H) = 0.35; P(θ_L) = 0.65

**Step 1: EU without information**

EU(D) = 0.35 × $15M + 0.65 × (−$8M) = $5.25M − $5.20M = **+$0.05M**
EU(ND) = 0.35 × $0 + 0.65 × $0 = $0

Optimal action: **Drill** (barely positive EU). EU* = $0.05M.

**Step 2: EU with perfect information**

- If θ_H revealed (prob 0.35): choose D, gain $15M
- If θ_L revealed (prob 0.65): choose ND, gain $0

EU_{PI} = 0.35 × $15M + 0.65 × $0 = **$5.25M**

**Step 3: EVPI**

```
EVPI = $5.25M − $0.05M = $5.20M
```

**Interpretation**: The company should pay up to **$5.20M** for a perfect seismic survey that reveals the true geology. Any survey costing less than $5.20M with some information value is potentially worth acquiring.

**Key insight**: The EU* is only barely positive ($0.05M) — the decision is nearly a tie. The gap between the committed-action value ($0.05M) and the flexible-response value ($5.25M) is large precisely because the existing information makes the decision roughly balanced (high prior uncertainty about which action is better).

---

## 1.3 Expected Value of Sample (Imperfect) Information (EVSI)

### 1.3.1 The Structure of Imperfect Information

A sample or test does not reveal θ perfectly; it produces a signal Z ∈ Z (the set of possible observations). The relationship between θ and Z is captured by the **likelihood function** p(Z = z | θ).

**Bayesian updating**: After observing Z = z:

```
p(θ | z) = p(z | θ) · p(θ) / p(z)    (Bayes' theorem)
```

where p(z) = ∫ p(z | θ) p(θ) dθ is the marginal likelihood of signal z.

### 1.3.2 Formal EVSI Derivation

With access to sample information, the decision-maker observes Z = z and then chooses:

```
a*(z) = argmax_{a ∈ A} E_θ[u(a, θ) | z] = argmax_{a ∈ A} ∫ u(a, θ) p(θ|z) dθ
```

The **expected utility with sample information** (averaging over the distribution of signals before observing z):

```
EU_{SI} = E_Z[max_{a ∈ A} E_θ[u(a, θ) | Z]]
        = ∫_Z [max_{a ∈ A} ∫_Θ u(a, θ) p(θ|z) dθ] p(z) dz
```

**Definition**: The Expected Value of Sample Information is:

```
EVSI ≡ EU_{SI} − EU*
```

This is a **preposterior analysis** — computed before observing Z. It is the expected improvement in decision quality from being able to observe Z (averaged over all possible signals).

**Key relationship**: EVSI ≤ EVPI always. Sample information is at best perfect information.

**Proof**: EVSI is the value when the clairvoyant reveals Z = z (imperfect about θ); EVPI is the value when the clairvoyant reveals θ directly. Since knowing θ gives strictly more decision-relevant information than knowing Z (which is only a signal about θ), EU_{PI} ≥ EU_{SI}, so EVPI ≥ EVSI. ∎

### 1.3.3 Decision to Acquire Information

Acquire the sample if and only if:

```
EVSI > Cost_of_sample
```

Or equivalently, if the Net Expected Value of Sample Information (NEVSI) is positive:

```
NEVSI = EVSI − Cost > 0
```

### 1.3.4 Worked Example: EVSI for the Oil Exploration Case

**Test**: A seismic survey costing $C produces signal Z ∈ {Positive, Negative}.

Likelihood function (survey reliability):
- P(Z = Pos | θ_H) = 0.80  (sensitivity: correctly identifies high-oil zones 80% of the time)
- P(Z = Neg | θ_H) = 0.20
- P(Z = Pos | θ_L) = 0.30  (false positive rate: incorrectly signals high when dry 30% of the time)
- P(Z = Neg | θ_L) = 0.70

**Step 1: Marginal probabilities of signals**

P(Z = Pos) = P(Pos|H)·P(H) + P(Pos|L)·P(L) = 0.80×0.35 + 0.30×0.65 = 0.28 + 0.195 = **0.475**
P(Z = Neg) = 0.20×0.35 + 0.70×0.65 = 0.07 + 0.455 = **0.525**

**Step 2: Posterior probabilities via Bayes**

Given Z = Pos:
P(H|Pos) = 0.28 / 0.475 = **0.589**
P(L|Pos) = 0.195 / 0.475 = **0.411**

Given Z = Neg:
P(H|Neg) = 0.07 / 0.525 = **0.133**
P(L|Neg) = 0.455 / 0.525 = **0.867**

**Step 3: Optimal action and EU in each signal state**

**If Z = Pos** (posterior: 58.9% High):
EU(D | Pos) = 0.589 × $15M + 0.411 × (−$8M) = $8.835M − $3.288M = **$5.547M**
EU(ND | Pos) = $0
→ Optimal: **Drill**; EU = $5.547M

**If Z = Neg** (posterior: 13.3% High):
EU(D | Neg) = 0.133 × $15M + 0.867 × (−$8M) = $1.995M − $6.936M = **−$4.941M**
EU(ND | Neg) = $0
→ Optimal: **Don't Drill**; EU = $0

**Step 4: EVSI computation**

```
EU_{SI} = P(Pos) × [EU optimal | Pos] + P(Neg) × [EU optimal | Neg]
        = 0.475 × $5.547M + 0.525 × $0
        = $2.635M

EVSI = EU_{SI} − EU* = $2.635M − $0.05M = $2.585M
```

**Decision**: Acquire the survey if Cost < $2.585M. At cost $2.0M, NEVSI = $0.585M > 0 → acquire.

**Comparison**: EVSI ($2.585M) < EVPI ($5.20M), as required.

**Why is EVSI so much lower than EVPI?** With only 80% sensitivity and 70% specificity, the survey still leaves substantial residual uncertainty. A negative survey only shifts P(H) from 0.35 → 0.133 (not to 0), so the ND choice after a negative result still involves some risk of foregoing a high-oil deposit.

---

## 1.4 Expected Value of Imperfect Information (EVII)

Howard (1966) introduced the EVII as a formalization for the case where the information source may be systematically biased or conditioned. In the most general form:

```
EVII = EU under imperfect information − EU*
```

where "imperfect information" can include:
- Noisy signals (as in EVSI above) — imperfect observation of θ
- Restricted signals — only partial state space is observed
- Biased oracles — systematic under/over-reporting

EVII ≤ EVSI ≤ EVPI always (adding noise or bias can only reduce the value of the information).

**General formula**:

```
EVII = E_Z[max_{a} E_θ[u(a, θ) | Z, bias model]] − EU*
```

The bias model enters through the likelihood p(z | θ, bias_parameters).

---

## 1.5 The Subadditivity Property

For two independent information sources Z₁ and Z₂:

```
VoI(Z₁ ∪ Z₂) ≤ VoI(Z₁) + VoI(Z₂)
```

**Proof sketch**: Information gains are subject to diminishing returns. The marginal value of a second source, given the first has already been observed, is less than or equal to its standalone value. Formally, this follows from the concavity of the max-EU operator in the signal distribution. ∎

**Practical implication**: Gathering two independent surveys provides at most (and typically less than) the sum of their individual VoIs. This limits the value of "redundant" information gathering.

---

## 1.6 Connection to Shannon Information Theory

Lindley (1956, *Annals of Mathematical Statistics*) proposed using **Shannon entropy reduction** as the utility function in Bayesian experimental design:

```
Information utility = H(p_prior) − E_Z[H(p_posterior | z)]
                    = I(Θ; Z)    (mutual information between parameters and data)
```

where H(p) = −∫ p(θ) log p(θ) dθ is the Shannon entropy.

**Lindley's criterion**: The optimal experiment (design) maximizes the expected information gain:

```
ξ* = argmax_{ξ ∈ Ξ} I(Θ; Z | experimental design ξ)
```

**Chaloner & Verdinelli (1995, *Statistical Science*, 13(3), 273–304)**: Extended this to cover a comprehensive review of Bayesian optimal experimental design criteria, including α-optimal designs, c-optimal designs, and D-optimal designs, all unifiable under the expected utility framework with different utility functions for different goals.

**Relationship to VoI**: When the utility function u(a, θ) is the logarithmic scoring rule, VoI is exactly equal to mutual information. For general utility functions, VoI captures the decision-relevant component of information — what matters is not total entropy reduction but entropy reduction on the decision-relevant dimension.

---

## 1.7 Key Citations

- Chaloner, K., & Verdinelli, I. (1995). Bayesian experimental design: A review. *Statistical Science*, 13(3), 273–304.
- Clemen, R. T., & Reilly, T. (2001). *Making Hard Decisions with Decision Tools.* Duxbury Press.
- Cover, T. M., & Thomas, J. A. (1991). *Elements of Information Theory.* Wiley.
- Howard, R. A. (1966). Information value theory. *IEEE Transactions on Systems Science and Cybernetics*, 2(1), 22–26.
- Lindley, D. V. (1956). On a measure of the information provided by an experiment. *Annals of Mathematical Statistics*, 27(4), 986–1005.
- Raiffa, H. (1968). *Decision Analysis: Introductory Lectures on Choices Under Uncertainty.* Addison-Wesley.
- Savage, L. J. (1954). *The Foundations of Statistics.* Wiley.
