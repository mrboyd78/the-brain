# Deliverable 2: The Proportionality Principle — When Deliberation Is Utility-Positive

> **Survey**: Formal Theory of Value of Information  
> **Topic**: Conditions under which additional deliberation effort produces positive expected utility  
> **Date Compiled**: April 2026

---

## 2.1 The Core Principle

The proportionality principle is the application of VoI logic to the deliberation decision itself: treating the decision to gather more information (to deliberate further) as itself a decision under uncertainty.

**The fundamental question**: Is the *cost of deliberation* (in time, money, cognitive effort, opportunity cost) justified by the *expected improvement in decision quality* that the deliberation will produce?

The formal answer: deliberation is utility-positive if and only if:

```
EVSI(additional deliberation) > Cost(additional deliberation)
```

This section derives this condition explicitly as a function of the four key parameters.

---

## 2.2 Formalizing the Deliberation Decision

### 2.2.1 Setting Up the Meta-Decision

Let the object-level decision be about action a ∈ A under uncertainty θ, as before. Define:

- **D** = the deliberation resource (time, analysis effort, analytical cost)
- **δ** = the amount of deliberation (a scalar or vector)
- **Z(δ)** = the signal produced by deliberation δ (a random variable)
- **p(Z(δ) | θ)** = the likelihood function for the deliberation output
- **Cost(δ)** = the cost of deliberation level δ (in utility-equivalent units)

The deliberation meta-decision is to choose δ to maximize:

```
V(δ) = EVSI(Z(δ)) − Cost(δ)
```

The optimal deliberation level δ* satisfies:

```
δ* = argmax_{δ ≥ 0} [EVSI(Z(δ)) − Cost(δ)]
```

And the **condition for positive deliberation** is:

```
V(δ*) > 0   ⟺   EVSI(Z(δ*)) > Cost(δ*)
```

### 2.2.2 The Four Parameter Functions

Deliberation value depends on four factors:

**Factor 1: Prior Uncertainty H(p)**

Let H(p) be the entropy of the prior distribution over the decision-relevant variable θ. Higher entropy → higher uncertainty → higher potential value of information.

When θ is binary (Bernoulli with probability p of one state):

```
H(p) = −p log₂(p) − (1−p) log₂(1−p)
```

H(p) is maximized at p = 0.5 (complete uncertainty) and zero at p ∈ {0,1} (certainty).

**Intuition**: If you are already certain the state is θ₁, no information changes your action, and EVSI = 0 regardless of stakes or deliberation quality.

**Factor 2: Decision Stakes S**

The stakes of the decision are the payoff spread — the difference between the best and worst possible outcomes:

```
S = max_{a,θ} u(a, θ) − min_{a,θ} u(a, θ)
```

Equivalently, define the stakes more precisely as the value of making the right choice vs. making the wrong one:

```
S(θ) = max_{a} u(a, θ) − u(a_prior_optimal*, θ)
```

This is the opportunity cost of not adapting your action to the true state θ.

**Factor 3: Cost of Deliberation C**

The cost function C(δ) is typically monotone increasing in deliberation effort. It can include:
- Direct costs: monetary cost of a study, test, or consultation
- Opportunity cost: value of other uses of the analyst's time
- Time cost: temporal discounting of delayed decisions
- Decision-delay costs: when deliberation delays commitment in time-sensitive environments

**Factor 4: Information Gain from Deliberation G(δ)**

The information gain measures how much deliberation of level δ is expected to reduce uncertainty:

```
G(δ) = I(Θ; Z(δ))    (mutual information between state and deliberation output)
```

Equivalently, in decision terms:

```
G(δ) = H(p_prior) − E_Z[H(p_posterior | Z(δ))]
```

G(δ) is bounded above by H(p) (the maximum possible reduction in entropy equals prior entropy) and bounded below by 0. A useless deliberation that produces a signal independent of θ has G(δ) = 0.

For most realistic deliberation processes, G(δ) exhibits **diminishing returns**:

```
∂G/∂δ > 0    (more deliberation generally produces more information)
∂²G/∂δ² ≤ 0   (but at decreasing marginal rate)
```

---

## 2.3 The Proportionality Condition — Full Derivation

### 2.3.1 The EVSI in Terms of Stakes and Information Gain

**Key relationship** (Howard, 1966; derivation via mutual information):

Under the approximation that the utility function is approximately linear in the decision-relevant uncertainty (holds exactly for linear utility and approximately for bounded risk aversion):

```
EVSI(Z) ≈ S · Φ(G(Z), H(p))
```

where Φ is a function of the information gain relative to prior entropy.

For the binary case with linear utilities (exact result):

```
EVPI = S · [p(1−p)]^{1/2} · √(2/π)    (for Gaussian normal approximation)
```

More precisely, for a binary decision with prior probability p on the high-state and payoff spread S:

```
EVPI = S · min(p, 1−p)
```

(This exact result holds for 2×2 problems: the value of perfect information equals the stakes times the probability of choosing the wrong action under the prior — which is min(p, 1−p).)

**Proof**: If p < 0.5, the optimal prior action is to choose the action better in the low-state. With probability p, the high state occurs, and the decision was wrong — cost = S. So EVPI = p × S = min(p, 1−p) × S. ∎

**Generalization for imperfect information**: Define the information quality q ∈ [0,1] such that q = 0 means the signal is uninformative and q = 1 means the signal is perfect. Then:

```
EVSI ≈ q · EVPI = q · S · min(p, 1−p)
```

This provides the explicit decomposition:

```
EVSI = (information quality) × (decision stakes) × (prior uncertainty measure)
```

### 2.3.2 The Proportionality Condition

Deliberation is utility-positive if and only if:

```
EVSI(Z(δ)) > Cost(δ)
q(δ) · S · min(p, 1−p) > Cost(δ)
```

Or rearranging:

```
q(δ) > Cost(δ) / [S · min(p, 1−p)]
```

This is the **critical information quality threshold**: deliberation is worth undertaking only if the ratio of cost to (stakes × uncertainty) is lower than the deliberation's information quality.

**Corollary**: If the cost rate (cost per unit of information quality) is roughly constant:

```
Cost(δ) = c · q(δ)    (linear cost in information quality)
```

Then deliberation is worthwhile iff:

```
c · q < q · S · min(p, 1−p)
c < S · min(p, 1−p)
```

This means: **deliberation is worth undertaking at all only if unit cost of information quality is less than stakes × uncertainty**. If it costs more per "information unit" than the decision is worth, no amount of deliberation is utility-positive.

### 2.3.3 The Optimal Deliberation Level

At the optimum, marginal cost equals marginal VoI:

```
∂[EVSI(Z(δ))]/∂δ = ∂Cost(δ)/∂δ
```

With EVSI ≈ q(δ) · EVPI and Cost(δ) = c · δ, and assuming q(δ) = q_max · (1 − e^{−δ/τ}) (logistic information accumulation with saturation constant τ):

```
Optimal deliberation δ*: q_max · (EVPI/τ) · e^{−δ*/τ} = c
→  δ* = τ · ln(q_max · EVPI / (c · τ))
```

This shows δ* is:
- **Increasing in stakes S** (through EVPI): higher stakes → more deliberation
- **Increasing in prior uncertainty min(p, 1−p)**: more uncertain priors → more deliberation
- **Decreasing in cost c**: more expensive information → less deliberation
- **Increasing in information saturation τ**: slower-accumulating information → longer deliberation

---

## 2.4 The Special Role of Prior Uncertainty (Sensitivity Analysis)

### 2.4.1 The Decision Sensitivity Region

Define the **decision sensitivity region** as the range of prior beliefs p for which the optimal action would change. This is formally:

```
DSR = {p : ∃ θ s.t. argmax_a E_θ[u(a,θ)|p] changes at p}
```

**Key result**: EVPI > 0 if and only if the current prior p is inside the DSR.

If p is outside the DSR — meaning the optimal action is the same for any feasible prior — then information cannot change the decision and EVPI = 0 by definition.

**For the binary case**: The DSR is the interval p ∈ (p_indifference − ε, p_indifference + ε) around the probability that makes the decision-maker exactly indifferent between actions. The closer p is to the indifference point, the larger EVPI.

### 2.4.2 Sensitivity to Uncertainty: EVPI as a Function of p

For the 2-action, 2-state problem with stakes S:

```
EVPI(p) = S · min(p, 1−p)
```

This function:
- = 0 at p = 0 or p = 1 (certainty; no information needed)
- Is maximized at p = 0.5: EVPI = 0.5S (complete uncertainty; information maximally valuable)
- Increases linearly from both ends to the center

**Implication**: As the prior moves toward certainty, the value of additional deliberation falls rapidly. A decision-maker who is 95% confident in their prior belief gains far less from deliberation than one who is 55% confident, even at identical stakes.

---

## 2.5 Temporal Dimension: Time-Sensitive Deliberation

When decisions have a deadline T and deliberation takes time t, the optimal deliberation time must account for:

1. **Information quality accumulates**: q(t) → q_max as t → ∞
2. **Decision delay has cost**: payoff is discounted by factor e^{−r·t} where r is the delay cost rate
3. **Opportunity cost**: other decisions may be delayed while deliberating

The objective becomes:

```
max_{t ∈ [0,T]} [q(t) · EVPI · e^{−r·t} − C(t)]
```

**Optimal stopping time t*** satisfies:

```
dq/dt · EVPI · e^{−r·t*} = r · q(t*) · EVPI · e^{−r·t*} + dC/dt
∂G/∂t · S · Uncertainty = (temporal cost rate) + (marginal cost of deliberation)
```

This is the continuous-time version of the proportionality condition and is the foundational equation of optimal stopping theory (see Deliverable 4).

---

## 2.6 Worked Example: Proportionality in a Medical Testing Decision

**Setup**: A physician must decide whether to prescribe Drug X (D) or not (ND). Uncertain quantity: whether the patient has condition C.

| Action | Condition C present (θ₁) | Condition C absent (θ₂) |
|---|---|---|
| Drug X (D) | +8 (remission) | −3 (side effects) |
| No Drug (ND) | −2 (untreated illness) | +1 (no harm) |

Prior: P(C present) = p = 0.40; P(C absent) = 0.60

**Step 1: EU without additional testing**

EU(D) = 0.40×8 + 0.60×(−3) = 3.2 − 1.8 = **1.4**
EU(ND) = 0.40×(−2) + 0.60×1 = −0.8 + 0.6 = **−0.2**

Optimal prior action: **Drug X** (EU = 1.4). EU* = 1.4.

**Step 2: EVPI**

EU_{PI} = 0.40×8 + 0.60×1 = 3.2 + 0.6 = **3.8**
EVPI = 3.8 − 1.4 = **2.4 utility units**

**Step 3: EVSI for a diagnostic test** (sensitivity = 0.85, specificity = 0.80)

P(Test+ | C) = 0.85; P(Test+ | ¬C) = 0.20
P(Test+) = 0.85×0.40 + 0.20×0.60 = 0.34 + 0.12 = 0.46
P(Test−) = 0.54

If Test+: P(C|+) = 0.34/0.46 = 0.739; P(¬C|+) = 0.261
EU(D|+) = 0.739×8 + 0.261×(−3) = 5.912 − 0.783 = 5.129
EU(ND|+) = 0.739×(−2) + 0.261×1 = −1.478 + 0.261 = −1.217
→ Optimal: Drug X; EU = 5.129

If Test−: P(C|−) = 0.06/0.54 = 0.111; P(¬C|−) = 0.889
EU(D|−) = 0.111×8 + 0.889×(−3) = 0.888 − 2.667 = −1.779
EU(ND|−) = 0.111×(−2) + 0.889×1 = −0.222 + 0.889 = 0.667
→ Optimal: No Drug; EU = 0.667

EU_{SI} = 0.46×5.129 + 0.54×0.667 = 2.359 + 0.360 = **2.719**
EVSI = 2.719 − 1.4 = **1.319 utility units**

**Step 4: Test cost threshold**

The test is worth ordering if its cost (in utility units) < 1.319. If 1 utility unit = $1,000, the test is worth up to **$1,319**.

**Deliberation proportionality check**:
- Prior uncertainty: min(0.40, 0.60) = 0.40 — substantial
- Stakes: payoff spread = 8 − (−3) = 11 units; or decision stakes between optimal and suboptimal choice ≈ 2.4/0.40 = 6 units of stakes
- Information quality: test has quality q ≈ EVSI/EVPI = 1.319/2.4 ≈ 0.55
- Cost threshold: ≈ 0.55 × 2.4 = 1.32 utility units

**Proportionality confirmed**: The test's cost threshold is proportional to (information quality) × (stakes) × (uncertainty) = 0.55 × 6 × 0.40 = 1.32 ✓

---

## 2.7 Key Citations

- Clemen, R. T., & Reilly, T. (2001). *Making Hard Decisions with Decision Tools.* Duxbury Press.
- Cover, T. M., & Thomas, J. A. (1991). *Elements of Information Theory.* Wiley.
- Howard, R. A. (1966). Information value theory. *IEEE Transactions on Systems Science and Cybernetics*, 2(1), 22–26.
- Lindley, D. V. (1956). On a measure of the information provided by an experiment. *Annals of Mathematical Statistics*, 27(4), 986–1005.
- Parmigiani, G., & Inoue, L. (2009). *Decision Theory: Principles and Approaches.* Wiley.
- Raiffa, H. (1968). *Decision Analysis.* Addison-Wesley.
- Raiffa, H., & Schlaifer, R. (1961). *Applied Statistical Decision Theory.* Harvard Business School.
