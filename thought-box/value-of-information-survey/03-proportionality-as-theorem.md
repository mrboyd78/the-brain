# Deliverable 3: "Effort Proportional to Difficulty × Stakes" — Formal Theorem Analysis

> **Survey**: Formal Theory of Value of Information  
> **Topic**: Evaluating the proportionality claim as a formal theorem vs. empirical heuristic  
> **Date Compiled**: April 2026

---

## 3.1 The Claim

The claim to be evaluated is:

> "Optimal deliberation effort is proportional to the difficulty of the decision times the stakes of the decision."

More formally: **δ* ∝ H(p) × S**, where H(p) is a measure of decision difficulty (prior entropy or proxies thereof) and S is the payoff spread.

This claim has both popular and professional currency: it is implicit in many consulting frameworks, explicit in some decision science textbooks, and is often cited as a practical rule of thumb for effort allocation.

The question is: under what conditions is this a precisely derived mathematical theorem? And when does it fail?

---

## 3.2 When the Proportionality Claim IS a Formal Theorem

### 3.2.1 Theorem Statement

**Theorem (Proportionality of VoI)**:

*Under the following conditions, the EVPI is proportional to the product of decision stakes S and prior uncertainty H:*

**(C1)**: The action space has exactly two alternatives: A = {a₁, a₂}.

**(C2)**: The utility function is linear in the uncertain state: u(aᵢ, θ) = cᵢ + sᵢ·θ for constants cᵢ, sᵢ.

**(C3)**: The prior distribution p(θ) is Gaussian: θ ~ N(μ₀, σ₀²).

**(C4)**: Stakes S is defined as the payoff spread between optimal decisions across the range of uncertainty.

*Under (C1)–(C4)*:

```
EVPI = S · σ₀ · φ(z*) / S = σ₀ · φ(z*)    where z* = (threshold − μ₀)/σ₀
```

and for the symmetric case (z* = 0, the prior is at the indifference threshold):

```
EVPI = S · σ₀ · φ(0) = S · σ₀ · (1/√(2π)) ≈ 0.399 · S · σ₀
```

Since σ₀ is proportional to H(p) (the entropy of a Gaussian is H = ½ ln(2πeσ₀²), monotone increasing in σ₀), we have:

```
EVPI ∝ S · σ₀ ∝ S × (measure of uncertainty)
```

**Proof**:

For two actions with linear utilities:
u(a₁, θ) = s₁ · θ + c₁
u(a₂, θ) = s₂ · θ + c₂

With s₁ > s₂ (action 1 has higher sensitivity to θ), action 1 is preferred when:
s₁·θ + c₁ ≥ s₂·θ + c₂
θ ≥ τ ≡ (c₂ − c₁)/(s₁ − s₂)    (the indifference threshold)

Prior optimal action: a₁ if E[θ] ≥ τ, else a₂.

EVPI = E_θ[max(u(a₁,θ), u(a₂,θ))] − max(EU(a₁), EU(a₂))

With θ ~ N(μ₀, σ₀²) and assuming μ₀ = τ (the maximally uncertain case where prior mean equals indifference threshold):

The region where action 2 is (ex-post) better: θ < τ = μ₀

```
EVPI = (s₁ − s₂) · ∫_{-∞}^{μ₀} (μ₀ − θ) · φ((θ−μ₀)/σ₀)/σ₀ dθ
     = (s₁ − s₂) · σ₀ · φ(0)
     = S · σ₀ / √(2π)
```

where S ≡ (s₁ − s₂) is the rate at which the payoff spread changes with θ (the stakes).

For general μ₀ ≠ τ, the formula generalizes to:

```
EVPI = S · σ₀ · [φ(z*) − z*(1−Φ(z*))]    where z* = (μ₀ − τ)/σ₀
```

When z* = 0 (maximum uncertainty): EVPI = S · σ₀ · φ(0) = S · σ₀/√(2π) ∝ S · σ₀. ∎

**This is the formal theorem underlying the proportionality claim**. It holds precisely under conditions C1–C4.

---

### 3.2.2 Extending to EVSI: The Proportionality is Preserved for Linear-Normal Models

Under the Linear-Normal model (Gaussian prior, Gaussian likelihood with known variance), the EVSI from n observations is:

```
EVSI(n) = S · σ_posterior(n) · φ(z*(n))
```

where σ_posterior(n) = σ₀/√(1 + n·σ₀²/σ_likelihood²) decreases with more observations.

The effort allocation problem then has the explicit solution:

```
δ*(n) = optimal n = [{(S/c)·φ(z*) · σ₀/σ_likelihood}² − 1] · σ_likelihood²/σ₀²
```

where c is the per-observation cost. This is directly proportional to (S/c)² — effort scales with the square of stakes-to-cost ratio and is decreasing in measurement variance. The proportionality holds.

---

## 3.3 When the Proportionality Claim FAILS

### 3.3.1 Failure Mode 1: Nonlinear Utility Functions (Risk Aversion)

Under risk aversion (concave utility function), the simple proportionality breaks down.

**Why**: A risk-averse agent is willing to pay more for information even in cases where expected stakes are lower, because information reduces variance and risk-averse agents pay a premium for variance reduction. The contribution of stakes to EVPI is no longer purely multiplicative.

**Formal illustration**: For a risk-averse agent with utility u(x) = −e^{−rx} (CARA utility with risk aversion coefficient r):

```
EU(a | information) ≠ linear function of stakes S
```

The EVPI under CARA utility depends on the *risk premium*, which is a nonlinear function of σ₀ and r. Specifically:

```
EVPI_risk_averse > EVPI_risk_neutral  (for same stakes and uncertainty)
```

The excess value (the risk premium component) does not scale proportionally with S — it scales with S² for small risks (quadratic risk premium) and accelerates for larger risks.

**Reference**: Pratt (1964, *Econometrica*, 32, 122–136) on risk aversion; Wakker (2010, *Prospect Theory*) for general utility effects.

### 3.3.2 Failure Mode 2: Multiple Actions (> 2)

With three or more actions, EVPI cannot be reduced to a simple product of stakes and uncertainty. The prior must be inside the "sensitivity region" for each pairwise alternative comparison simultaneously, producing interaction effects that destroy the clean proportionality.

**Example**: With three actions and a 3-region prior space, EVPI depends on:
- Which region of the prior the current belief occupies
- The number of "switches" perfect information would produce
- The payoff differences across multiple action-state pairs simultaneously

The resulting EVPI is a sum of terms of the form min(pᵢ, ...) × Sᵢⱼ across all pairwise comparisons, not a simple product.

### 3.3.3 Failure Mode 3: Non-Gaussian Priors

The exact proportionality EVPI ∝ S·σ requires Gaussian priors. For fat-tailed priors (t-distribution, stable distributions, Power-law), the EVPI is higher than the Gaussian formula predicts — because rare but high-stakes states contribute disproportionately. The proportionality breaks in the tail.

**Implication**: In domains with fat-tailed outcome distributions (financial risk, catastrophic project failures, black-swan events), the Gaussian proportionality formula *systematically underestimates* EVPI and therefore underestimates the optimal deliberation effort. Organizations using the Gaussian proportionality heuristic in fat-tailed domains will systematically under-invest in information gathering.

### 3.3.4 Failure Mode 4: Correlated Alternatives and Dependent Information Sources

When gathering information on one criterion changes the posterior for another correlated criterion (common in multi-attribute decisions), the subadditivity property applies in a non-trivial way. The marginal VoI of each piece of information depends on what else has been gathered, and the simple product structure fails.

**Example**: In a hiring decision where technical skill and communication ability are correlated, gathering information on technical skill reduces uncertainty about communication ability as well. The optimal information gathering strategy must account for these correlations, and simple proportionality to individual attribute stakes fails.

### 3.3.5 Failure Mode 5: Irreversibility and Option Value

The proportionality claim assumes the decision can be optimized at a single point in time. When decisions have sequential structure — committing early forecloses later options — the dynamic option value of delay must be added.

**Option value of waiting**: Dixit & Pindyck (1994, *Investment Under Uncertainty*) showed that for irreversible decisions:

```
Optimal action threshold = VoI threshold × (1 + option_value_of_waiting)
```

The option value component is not proportional to current stakes and uncertainty — it depends on the stochastic process governing how stakes and uncertainty evolve over time. Higher volatility in future states increases option value non-linearly.

**Result**: For irreversible decisions under evolving uncertainty, the optimal deliberation effort is higher (and has different functional form) than the simple proportionality formula predicts.

---

## 3.4 Summary: When Proportionality Holds vs. Fails

| Condition | Proportionality Status | Notes |
|---|---|---|
| 2-action, linear utility, Gaussian prior | **Exact theorem** | EVPI = S·σ₀/√(2π) at max uncertainty |
| 2-action, linear utility, non-Gaussian prior | **Approximate heuristic** | Good approximation if prior is unimodal and roughly symmetric |
| >2 actions, any utility | **Fails as exact theorem** | Multiple interaction terms; no clean product form |
| Risk-averse utility, any action set | **Fails** | Additional risk-premium terms that scale nonlinearly with stakes |
| Correlated attributes | **Fails** | Marginal VoI depends on other information gathered |
| Irreversible decisions, evolving uncertainty | **Fails** | Option value adds non-proportional component |
| Fat-tailed distributions | **Underestimates** | Gaussian approximation misses tail contributions |
| Sequential, repeated decisions | **Approximate** | Gittins index characterization; see Deliverable 4 |

---

## 3.5 The Claim as an Empirical Heuristic

When the exact theorem conditions fail, the proportionality claim survives as a **direction-of-effect heuristic**: increases in difficulty (uncertainty) or stakes should generally lead to more deliberation. This directional claim is robust across all of the failure modes above, because:

1. Whether utility is linear or risk-averse, higher σ₀ increases EVPI
2. Whether there are 2 or more actions, higher stakes (measured as payoff spread) increase EVPI
3. Whether the prior is Gaussian or fat-tailed, the qualitative monotone relationships hold

**What fails** is the specific functional form (linear product) — not the directional claim. The heuristic "more uncertainty and higher stakes → more deliberation" is always directionally correct. The specific quantity of optimal additional deliberation requires full VoI calculation when the conditions of the theorem are violated.

---

## 3.6 Key Citations

- Dixit, A. K., & Pindyck, R. S. (1994). *Investment Under Uncertainty.* Princeton University Press.
- Howard, R. A. (1966). Information value theory. *IEEE Transactions on Systems Science and Cybernetics*, 2(1), 22–26.
- Lindley, D. V. (1956). On a measure of the information provided by an experiment. *Annals of Mathematical Statistics*, 27(4), 986–1005.
- Pratt, J. W. (1964). Risk aversion in the small and in the large. *Econometrica*, 32(1–2), 122–136.
- Raiffa, H., & Schlaifer, R. (1961). *Applied Statistical Decision Theory.* Harvard Business School.
- Wakker, P. P. (2010). *Prospect Theory: For Risk and Ambiguity.* Cambridge University Press.
