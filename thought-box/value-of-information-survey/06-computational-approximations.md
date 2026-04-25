# Deliverable 6: Computational Approximations — Tractable VoI Heuristics

> **Survey**: Formal Theory of Value of Information  
> **Topic**: Practical methods for when exact VoI is computationally intractable  
> **Date Compiled**: April 2026

---

## 6.1 The Complexity Challenge

Exact computation of Value of Information (VoI) requires "Preposterior Analysis": predicting all possible experimental outcomes, Bayes-updating for each, finding the new optimal action for each, and averaging the results.

In complex, multi-stage, or high-dimensional problems, this leads to an **Exponential Explosion** (the "Curse of Dimensionality"). For example, in a sensor network with $K$ nodes, calculating the joint VoI of all subsets has $2^K$ complexity. 

This deliverable summarizes the heuristics and approximations used to allocate effort when the exact optimum is out of reach.

---

## 6.2 Myopic (One-Step) VoI

The most common approximation in sequential information gathering is the **Myopic Heuristic**.

### 6.2.1 The Heuristic
Instead of calculating the value of the entire future information-gathering sequence, calculate only the value of the *immediate next* observation as if it were the last one possible.

- **Stop condition**: Stop if $\text{EVSI}(\text{next observation}) < \text{Cost}(\text{next observation})$.
- **Selection**: Pick the next observation $z$ that maximizes $\text{EVSI}(z) - \text{Cost}(z)$.

### 6.2.2 Critical Failure: "Greedy Discontent"
The myopic heuristic can fail catastrophically in problems with **complementary information**.
- **The XOR Problem**: If knowing $A$ alone tells you nothing, and $B$ alone tells you nothing, but $(A, B)$ together resolve the decision, myopic VoI will assign zero value to both. A myopic agent will prematurely stop, whereas a long-term agent would see the joint value.

---

## 6.3 Monte Carlo and Proxy Approximations

When the integral over signals $Z$ is intractable, designers use sampling.

### 6.3.1 Expected Information Gain (EIG) Proxy
Instead of EVSI (which is decision-specific), use **Shannon Mutual Information** $I(\Theta; Z)$ as a proxy.
- **Why?**: $I(\Theta; Z)$ is often easier to approximate via Monte Carlo Dropout or Variational Inference.
- **Bounded Rationality**: While EVSI is perfectly "rational" for a single decision, EIG is often more "robust" across multiple related decisions.

### 6.3.2 Upper Confidence Bound (UCB)
In Bandit problems, instead of the complex Gittins Index, the UCB heuristic is used:
$$\text{Action}_t = \text{argmax} \left[ \hat{\mu}_i + \alpha \sqrt{\frac{\ln t}{n_i}} \right]$$
This approximates the VoI by adding an "uncertainty bonus" (the square root term). It is computationally trivial and achieves near-optimal regret bounds (Auer et al., 2002).

---

## 6.4 Sensitivity Analysis as a Filter

A powerful heuristic in corporate decision analysis (Clemen & Reilly) is to use **Deterministic Sensitivity Analysis** before any VoI calculation.

1. **The Tornado Diagram**: Vary each uncertain parameter $\theta_i$ between its 10th and 90th percentile while holding others constant.
2. **The "Change in Action" Filter**: If the optimal action $a^*$ remains the same across the entire range of $\theta_i$, then $\text{EVPI}(\theta_i) = 0$.
3. **Execution**: Perform expensive EVSI calculations *only* for the 1–3 variables that are "decision-sensitive."

---

## 6.5 The "Metacognitive" Stopping Heuristic

For purely cognitive deliberation (e.g., "how much longer should I think about this?"), the exact computation of "potential change in mental state" is recursive and impossible.

**The "Stability" Heuristic**:
Identify the current proposed action $a^*_n$. Continue deliberation until the identity of $a^*_n$ remains stable for some period $\Delta t$ or through several different "mental simulations." If the proposed action is oscillating, the EVSI of further thought is high. If it has converged, the marginal value of further thought is likely lower than the opportunity cost of time.

---

## 6.6 Summary of Heuristic Applicability

| Heuristic | Best Use Case | Failure Mode |
|---|---|---|
| **Myopic VoI** | Independent sensors/tests | Complementary/synergistic data |
| **Tornado/Sensitivity** | Initial scoping of large projects | Correlated parameters (interactions) |
| **UCB / Thompson Sampling** | Repeated trials, web optimization | Non-stochastic / non-stationarity |
| **Entropy Proxy** | Deep learning, high-dim modeling | Misalignment with decision stakes |
| **SPRT Thresholding** | Quality control, binary diagnosis | Multiple hypotheses |

---

## 6.7 Key Citations

- Auer, P., Cesa-Bianchi, N., & Fischer, P. (2002). Finite-time analysis of the multiarmed bandit problem. *Machine Learning*, 47(2), 235-256.
- Clemen, R. T., & Reilly, T. (2013). *Making Hard Decisions with DecisionTools*. South-Western Cengage Learning.
- Krause, A., & Guestrin, C. (2009). Optimal Value of Information in Graphical Models. *Journal of Artificial Intelligence Research*, 35, 557-591.
- Russell, S., & Wefald, E. (1991). *Do the Right Thing: Studies in Limited Rationality*. MIT Press.
