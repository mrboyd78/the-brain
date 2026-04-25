# Calibration Drift Detection: Statistical Methods Survey
## Executive Summary

**Survey Date:** April 2026  
**Topic:** Statistical methods for detecting calibration drift in repeated forecasting or classification tasks, with emphasis on required observation counts  
**Scope:** Classical hypothesis testing, sequential analysis, change-point detection, Bayesian methods, applied calibration literature

---

## What Is Calibration Drift?

A forecaster or classifier is **well-calibrated** if, among all predictions assigned probability *p*, the long-run empirical frequency of positive outcomes is also *p*. A system that reports 70% confidence on questions that resolve correctly only 55% of the time is **miscalibrated by 15 percentage points**.

**Calibration drift** is the temporal phenomenon where a system that was initially well-calibrated becomes miscalibrated over time due to:
- Changes in the distribution of inputs (covariate shift)
- Changes in the true underlying outcome probabilities (concept drift)
- Degrading model performance (model decay)
- Systematic biases accumulating in the reasoning process

The statistical challenge: distinguishing genuine drift from expected sampling variability, and doing so quickly enough to be actionable before the miscalibration causes harm.

---

## The Four Core Statistical Approaches

| Approach | Best Use Case | Requires Fixed N? | Controls Type I Error? |
|---|---|---|---|
| **Classical hypothesis testing** | Periodic audits with predetermined N | Yes | Yes (exactly) |
| **Sequential analysis (SPRT, GSD)** | Continuous monitoring, early stopping | No | Yes (by design) |
| **Change-point detection (CUSUM, EWMA)** | Ongoing surveillance, temporal drift | No | Via ARL tuning |
| **Sequential Bayes factors** | Evidence accumulation, interpretable stopping | No | Approximately |

---

## Quick-Reference Numbers

| Drift Magnitude | Binary outcome (p₀=0.50) | Binary outcome (p₀=0.70) | 5-bucket graded |
|---|---|---|---|
| 10 pp drift | N ≈ 783 (fixed) / ~550 (SPRT) | N ≈ 868 | N ≈ 900–1200 |
| 20 pp drift | N ≈ 197 (fixed) / ~135 (SPRT) | N ≈ 214 | N ≈ 250–350 |
| 30 pp drift | N ≈ 87 (fixed) / ~60 (SPRT) | N ≈ 93 | N ≈ 110–160 |

*All at α=0.05, power=0.80, one-sided. Full derivations in Deliverable 1.*

---

## Deliverables Index

| File | Deliverable |
|---|---|
| `01-minimum-n-formulas.md` | Sample size formulas, tables, worked examples |
| `02-sequential-vs-fixed-n.md` | Sequential vs. fixed-N comparison and efficiency |
| `03-changepoint-detection.md` | CUSUM and EWMA detection delays |
| `04-recommended-protocol.md` | Implementable monitoring protocol |
| `05-failure-modes.md` | False alarms, missed drift, failure regimes |
| `06-citations.md` | Full citations |

---

## Key Finding: The Observation Count Problem

The survey's most critical empirical finding for practitioners: **detecting 10 percentage-point miscalibration in a single confidence bucket with 80% power requires approximately 800 observations in that bucket**. For a system with 5 confidence buckets, this means roughly 4,000 observations just to audit each bucket once at this precision. Sequential methods (SPRT, CUSUM) reduce this by approximately 30–50% on average under favorable conditions but do not fundamentally escape the fundamental statistical limits imposed by the signal-to-noise ratio of the calibration error.

The practical implication: **calibration monitoring is inherently a slow process for small drift magnitudes**. A 30pp drift — the kind of miscalibration that is immediately obvious to careful inspection — requires only ~90 observations. This is the regime where quick feedback is possible. For subtle 10pp drift, even the most efficient sequential methods require hundreds of observations per confidence tier.
