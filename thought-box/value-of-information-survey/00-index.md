# Value of Information — Formal Theory Survey — Index

> **Topic**: When additional analysis or deliberation is worth its cost  
> **Date Compiled**: April 2026

---

## Deliverables

| # | File | Topic |
|---|---|---|
| 1 | [01-voi-canonical-formulas.md](./01-voi-canonical-formulas.md) | EVPI, EVSI, EVII — derivations and worked examples |
| 2 | [02-proportionality-principle.md](./02-proportionality-principle.md) | When deliberation effort is utility-positive |
| 3 | [03-proportionality-as-theorem.md](./03-proportionality-as-theorem.md) | "Effort ∝ difficulty × stakes" — formal theorem analysis |
| 4 | [04-optimal-stopping.md](./04-optimal-stopping.md) | When to stop gathering information and decide |
| 5 | [05-empirical-deviations.md](./05-empirical-deviations.md) | Analysis paralysis, premature closure, mis-allocation |
| 6 | [06-computational-approximations.md](./06-computational-approximations.md) | Tractable VoI heuristics for complex problems |

---

## Theoretical Lineage

```
Savage (1954) — Subjective Expected Utility foundations
    └── Howard (1966) — Information Value Theory (decision-theoretic VoI)
        └── Raiffa (1968) — Decision Analysis (EVPI/EVSI taxonomy)
            ├── Lindley (1956) — Bayesian experimental design criterion
            │   └── Chaloner & Verdinelli (1995) — Optimal design review
            └── Clemen & Reilly (2001) — Applied decision analysis
                └── Gittins (1979) — Bandit index / optimal stopping
                    └── Cover & Thomas (1991) — Information-theoretic formulations
```

---

## Core Properties of VoI (All Derivations Prove These)

| Property | Statement | Proof Location |
|---|---|---|
| **Non-negativity** | EVPI ≥ 0; EVSI ≥ 0; EVII ≥ 0 always | §1.3 |
| **EVPI ≥ EVSI** | Perfect information is worth at least as much as any sample | §1.4 |
| **Zero-value condition** | EVPI = 0 iff the optimal action does not change under any state | §1.3 |
| **Dominance** | If preferred action dominates all states, EVPI = 0 | §1.3 |
| **Subadditivity** | VoI from two independent signals ≤ sum of their individual VoIs | §1.5 |
| **Monotone in stakes** | VoI scales with payoff spread — doubling stakes doubles EVPI | §2.3 |
| **Proportionality** | Effort positive iff EVSI > cost; rate ∝ (prior H × stakes × reliability) | §2 |
