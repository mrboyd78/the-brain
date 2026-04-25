# Deliverable 4: Optimal Stopping — When to Decide

> **Survey**: Formal Theory of Value of Information  
> **Topic**: Formal conditions for stopping information search; Sequential sampling and Bandit theory  
> **Date Compiled**: April 2026

---

## 4.1 The Stopping Problem: "Decide Now vs. Observe More"

Optimal stopping is the temporal boundary of VoI. In the canonical static case, we decide whether to buy a fixed batch of information. In the **sequential case**, the decision-maker observes data points one by one and must decide at each step whether the accumulated evidence is sufficient to act or if the marginal value of *one more* observation exceeds its cost.

The decision-maker faces a triple trade-off:
1. **Decision Error Cost**: Utility lost by choosing a suboptimal action due to residual uncertainty.
2. **Observation Cost**: Direct resource cost (time, money, effort) per information unit.
3. **Opportunity Cost**: Payoffs foregone during the delay (time value of money or competitive pre-emption).

---

## 4.2 Sequential Probability Ratio Test (SPRT)

Wald (1947) provided the foundational result for the simplest sequential stopping problem: choosing between two simple hypotheses ($H_0$ vs $H_1$) based on a stream of independent observations $x_1, x_2, ...$.

### 4.2.1 The Likelihood Ratio
At step $n$, we compute the likelihood ratio:
$$\Lambda_n = \prod_{i=1}^n \frac{P(x_i | H_1)}{P(x_i | H_0)}$$

### 4.2.2 The Optimal Stopping Rule
The decision rule is defined by two thresholds $A$ and $B$ (where $0 < B < 1 < A$):
- If $\Lambda_n \ge A$: Stop and Accept $H_1$.
- If $\Lambda_n \le B$: Stop and Accept $H_0$.
- If $B < \Lambda_n < A$: Continue observing.

**Optimality Property**: Wald and Wolfowitz (1948) proved that for given error probabilities $\alpha$ (False Positive) and $\beta$ (False Negative), the SPRT minimizes the expected number of observations $E[n]$.

---

## 4.3 The Dynamic Programming Formulation

In a general Bayesian setting with action space $A$ and state $\theta$, the state of our knowledge at step $n$ is the posterior $p_n(\theta)$. Let $V(p_n)$ be the maximum expected utility from this point forward.

**The Bellman Equation for Optimal Stopping**:
$$V(p_n) = \max \left[ \text{EU}^*(p_n), \quad E_{z|p_n} [V(p_{n+1})] - C \right]$$

Where:
- $\text{EU}^*(p_n)$ is the expected utility of deciding immediately based on current beliefs.
- $E_{z|p_n} [V(p_{n+1})] - C$ is the expected future utility of taking one more observation at cost $C$ and then acting optimally from the resulting new state.

### 4.3.1 The Stopping Condition
An agent stops gathering information when:
$$\text{EU}^*(p_n) \ge E_{z|p_n} [V(p_{n+1})] - C$$

**Interpretation**: Stop when the "option value" of the next observation (the expected improvement in future utility) is less than or equal to the cost of that observation.

---

## 4.4 Multi-Armed Bandits and the Gittins Index

When information gathering is not just a precursor to a decision but integrated into a sequence of repeated actions (e.g., trying different clinical treatments or exploring oil wells), we face the **Explore-Exploit** trade-off.

### 4.4.1 The Theorem (Gittins, 1979)
For a multi-armed bandit problem with discount factor $\gamma < 1$, the optimal strategy is to always pull the arm with the highest **Gittins Index** $\nu_i$.

### 4.4.2 The Index Formulation
The Gittins Index for an arm is the solution to an auxiliary optimal stopping problem:
$$\nu_i = \sup_{\tau > 0} \frac{E \left[ \sum_{t=0}^{\tau-1} \gamma^t R_t \right]}{E \left[ \sum_{t=0}^{\tau-1} \gamma^t \right]}$$

Where $\tau$ is a stopping time. The index $\nu_i$ effectively measures the "fair price" of an arm, incorporating both its expected immediate reward and its "information value" (potential to prove better than it looks).

**Stopping Implications**:
- We stop pulling an arm (explore) and switch to another (exploit) when its Gittins index drops below that of a competing option.
- Arms with high uncertainty (low sample size) get an "exploration bonus" in their index, making it rational to "over-sample" them relative to their current mean reward.

---

## 4.5 Optimal Stopping in Finite Time (Secretary Problem)

In some contexts, the cost of information is not a continuous rate but a **rank-order constraint** (e.g., you must choose one of $N$ candidates, and once rejected, a candidate cannot be recalled).

**The $1/e$ Rule**: To maximize the probability of selecting the single best candidate from $N$:
1. Observe and reject the first $N/e \approx 37\%$ of candidates.
2. Set their best score as your "baseline."
3. Select the next candidate who outperforms the baseline.

**Formal Condition**: Stop when the probability of the current option being the global optimum exceeds the expected probability of finding a better one in the remaining $N-n$ samples.

---

## 4.6 Rational Stopping as a Function of Stakes

For a search process over an unknown distribution of payoffs (e.g., searching for a lower price or a better employee), the stopping condition is:

**Stop if current value $x \ge$ Reservation Value $x^*$**
Where $x^*$ satisfies:
$$\text{Cost of one more search} = \int_{x^*}^\infty (y - x^*) f(y) dy$$

**The Principle of Limited Search**: 
1. **High Stakes (Stakes $\uparrow$)**: The integral on the right is "stretched." To keep the equality with fixed cost, $x^*$ must increase. We deliberate *longer*.
2. **High Uncertainty (Variance $\uparrow$)**: The tail of $f(y)$ is thicker. To keep the equality, $x^*$ must increase. We deliberate *longer*.

---

## 4.7 Key Citations

- Gittins, J. C. (1979). Bandit processes and dynamic allocation indices. *Journal of the Royal Statistical Society: Series B (Methodological)*, 41(2), 148-164.
- Wald, A. (1947). *Sequential Analysis*. Wiley.
- DeGroot, M. H. (1970). *Optimal Statistical Decisions*. McGraw-Hill.
- Ferguson, T. S. (1989). Who solved the secretary problem? *Statistical Science*, 4(3), 282-289.
