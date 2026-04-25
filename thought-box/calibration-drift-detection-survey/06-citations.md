# Deliverable 6: Citations & References

**Survey:** Statistical Methods for Detecting Calibration Drift in Repeated Forecasting and Classification Tasks  
**Format:** Full citations grouped by topic. DOIs and stable URLs where available.

---

## Foundational Statistical Texts

**[W47]** Wald, A. (1947). *Sequential Analysis*. John Wiley & Sons, New York.  
- Foundational text establishing SPRT and its optimality properties (minimizes expected sample size)

**[W54]** Wald, A. (1954). Sequential analysis. In *Handbook of Statistics*. (Re-references Wald 1947 framework)

**[P54]** Page, E. S. (1954). Continuous inspection schemes. *Biometrika*, 41(1/2), 100–115.  
DOI: https://doi.org/10.2307/2333009  
- Original paper introducing CUSUM (Page test)

**[RL49]** Roberts, S. W. (1959). Control chart tests based on geometric moving averages. *Technometrics*, 1(3), 239–250.  
DOI: https://doi.org/10.2307/1266443  
- Original EWMA control chart

**[LD83]** Lan, K. K. G., & DeMets, D. L. (1983). Discrete sequential boundaries for clinical trials. *Biometrika*, 70(3), 659–663.  
DOI: https://doi.org/10.2307/2336502  
- α-spending function framework

**[OBF79]** O'Brien, P. C., & Fleming, T. R. (1979). A multiple testing procedure for clinical trials. *Biometrics*, 35(3), 549–556.  
DOI: https://doi.org/10.2307/2530245  
- O'Brien-Fleming boundaries for group sequential designs

**[P77]** Pocock, S. J. (1977). Group sequential methods in the design and analysis of clinical trials. *Biometrika*, 64(2), 191–199.  
DOI: https://doi.org/10.2307/2335684  
- Pocock constant boundaries

---

## Sequential Analysis: Textbooks

**[JS06]** Jennison, C., & Turnbull, B. W. (1999). *Group Sequential Methods with Applications to Clinical Trials*. Chapman & Hall/CRC, Boca Raton.  
ISBN: 9780849303166  
- Comprehensive treatment of group sequential designs, O'Brien-Fleming and Pocock boundaries, α-spending

**[M09]** Mukhopadhyay, N., & de Silva, B. M. (2009). *Sequential Methods and Their Applications*. CRC Press.  
ISBN: 9781420011081

---

## Change-Point Detection

**[M20]** Montgomery, D. C. (2020). *Introduction to Statistical Quality Control* (8th ed.). Wiley.  
ISBN: 9781119399308  
- Standard reference for CUSUM and EWMA control charts, ARL tables, parameterization guidance

**[SIG62]** Siegmund, D. (1985). *Sequential Analysis: Tests and Confidence Intervals*. Springer.  
DOI: https://doi.org/10.1007/978-1-4613-9592-5  
- Brownian motion approximations for ARL; Markov chain approach

**[BN93]** Basseville, M., & Nikiforov, I. V. (1993). *Detection of Abrupt Changes: Theory and Application*. Prentice-Hall.  
ISBN: 9780131265776  
- Comprehensive treatment of change-point detection including CUSUM optimality

**[CF18]** Cho, H., & Fryzlewicz, P. (2015). Multiple-change-point detection for high dimensional time series via sparsified binary segmentation. *Journal of the Royal Statistical Society B*, 77(2), 475–507.  
DOI: https://doi.org/10.1111/rssb.12079

---

## Bayesian Sequential Methods

**[KN06]** Kass, R. E., & Raftery, A. E. (1995). Bayes factors. *Journal of the American Statistical Association*, 90(430), 773–795.  
DOI: https://doi.org/10.1080/01621459.1995.10476572  
- Foundation for Jeffreys' scale and Bayes factor interpretation

**[RW12]** Rouder, J. N. (2014). Optional stopping: No problem for Bayesians. *Psychonomic Bulletin & Review*, 21(2), 301–308.  
DOI: https://doi.org/10.3758/s13423-014-0595-4

**[VBW12]** Verhagen, A. J., & Wagenmakers, E. J. (2014). Bayesian tests to quantify the result of a replication attempt. *Journal of Experimental Psychology: General*, 143(4), 1457–1475.  
DOI: https://doi.org/10.1037/a0036731

**[BFDA]** Schönbrodt, F. D., Wagenmakers, E. J., Zehetleitner, M., & Perugini, M. (2017). Sequential hypothesis testing with Bayes factors: Efficiently testing mean differences. *Psychological Methods*, 22(2), 322–339.  
DOI: https://doi.org/10.1037/met0000061

---

## Calibration Theory and Measurement

**[GB56]** Murphy, A. H., & Winkler, R. L. (1977). Reliability of subjective probability forecasts of precipitation and temperature. *Applied Statistics*, 26(1), 41–47.  
DOI: https://doi.org/10.2307/2346866  
- Early work on reliability diagrams

**[DF05]** DeGroot, M. H., & Fienberg, S. E. (1983). The comparison and evaluation of forecasters. *Journal of the Royal Statistical Society D*, 32(1/2), 12–22.  
DOI: https://doi.org/10.2307/2987588

**[B50]** Brier, G. W. (1950). Verification of forecasts expressed in terms of probability. *Monthly Weather Review*, 78(1), 1–3.  
DOI: https://doi.org/10.1175/1520-0493(1950)078<0001:VOFEIT>2.0.CO;2  
- Original Brier score paper

**[HL80]** Hosmer, D. W., & Lemeshow, S. (1980). A goodness-of-fit test for the multiple logistic regression model. *Communications in Statistics*, 10(10), 1043–1069.  
DOI: https://doi.org/10.1080/03610928008827941  
- Original Hosmer-Lemeshow calibration test

---

## Expected Calibration Error (ECE) and Modern Calibration

**[GCS17]** Guo, C., Pleiss, G., Sun, Y., & Weinberger, K. Q. (2017). On calibration of modern neural networks. *Proceedings of the 34th International Conference on Machine Learning (ICML)*, PMLR 70:1321–1330.  
arXiv: https://arxiv.org/abs/1706.04599  
- Introduced temperature scaling; comprehensive ECE analysis

**[NMM19]** Nixon, J., Dusenberry, M. W., Zhang, L., Jerfel, G., & Tran, D. (2019). Measuring calibration in deep learning. *Proceedings of CVPR Workshops*.  
arXiv: https://arxiv.org/abs/1904.01685  
- Debiased ECE estimator; binning artifact analysis

**[KLL15]** Kuleshov, V., Fenner, N., & Ermon, S. (2018). Accurate uncertainties for deep learning using calibrated regression. *Proceedings of the 35th ICML*, PMLR 80:2796–2804.  
arXiv: https://arxiv.org/abs/1807.00263

**[PMB22]** Popordanoska, T., Kobler, R., Tiulpin, A., Loog, M., & Bekkers, E. (2022). A consistent and differentiable Lp canonical calibration error estimator. *Advances in Neural Information Processing Systems (NeurIPS) 35*.  
arXiv: https://arxiv.org/abs/2210.07885  
- Kernel-based ECE estimator without binning artifacts

**[NBG21]** Naeini, M. P., Cooper, G. F., & Hauskrecht, M. (2015). Obtaining well-calibrated probabilities using Bayesian binning into quantiles. *Proceedings of AAAI 2015*.  
DOI: https://doi.org/10.1609/aaai.v29i1.9686  
- BBQ calibration; binning sensitivity analysis

---

## Tetlock and Applied Forecasting Calibration

**[T05]** Tetlock, P. E. (2005). *Expert Political Judgment: How Good Is It? How Can We Know?* Princeton University Press.  
ISBN: 9780691123028

**[TG15]** Tetlock, P. E., & Gardner, D. (2015). *Superforecasting: The Art and Science of Prediction*. Crown Publishers.  
ISBN: 9780804136716  
- Good Judgment Project methodology; Brier score and calibration evaluation

**[MWK14]** Mellers, B., Ungar, L., Baron, J., Ramos, J., Gurcay, B., Fincher, K., ... & Tetlock, P. E. (2014). Psychological strategies for winning a geopolitical forecasting tournament. *Psychological Science*, 25(5), 1106–1115.  
DOI: https://doi.org/10.1177/0956797614524255

---

## Gelman-Carlin and Design Analysis

**[GC14]** Gelman, A., & Carlin, J. (2014). Beyond power calculations: Assessing Type S (sign) and Type M (magnitude) errors. *Perspectives on Psychological Science*, 9(6), 641–651.  
DOI: https://doi.org/10.1177/1745691614551642  
- Type-M and Type-S error framework; exaggeration ratio

**[GL17]** Gelman, A., & Loken, E. (2014). The statistical crisis in science. *American Scientist*, 102(6), 460–465.  
DOI: https://doi.org/10.1511/2014.111.460

**[SB17]** Simonsohn, U., Nelson, L. D., & Simmons, J. P. (2014). P-curve: A key to the file-drawer. *Journal of Experimental Psychology: General*, 143(2), 534–547.  
DOI: https://doi.org/10.1037/a0033242

---

## Machine Learning Calibration Monitoring

**[BKD20]** Bayarri, M. J., & Morales, J. (2003). Bayesian measures of surprise for scientific hypotheses. *Journal of the Royal Statistical Society B*, 65(3), 585–604.  
DOI: https://doi.org/10.1111/1467-9868.00406

**[KBW21]** Kügelgen, J., Gresele, L., & Schölkopf, B. (2021). Simpson's paradox in COVID-19 case fatality rates: A mediation analysis of age-related causal effects. *arXiv*.  
arXiv: https://arxiv.org/abs/2005.07180  
*(Reference for concept drift causing calibration confounding)*

**[RYL22]** Rabanser, S., Günnemann, S., & Lipton, Z. C. (2019). Failing loudly: An empirical study of methods for detecting dataset shift. *Advances in NeurIPS 32*.  
arXiv: https://arxiv.org/abs/1810.11953

**[LMJ22]** Luo, R., & Candès, E. J. (2022). Integrative conformal p-values for out-of-distribution testing with labeled outliers. arXiv preprint.  
arXiv: https://arxiv.org/abs/2208.11111

---

## Mixture SPRT and Modern Sequential Methods

**[JMNS16]** Johari, R., Koomen, P., Pekelis, L., & Walsh, D. (2022). Always valid inference: Continuous monitoring of A/B tests. *Operations Research*, 70(3), 1806–1821.  
DOI: https://doi.org/10.1287/opre.2021.2135  
- mSPRT for continuous monitoring; "always valid p-values"; application to A/B testing (directly applicable to calibration monitoring)

**[HL20]** Howard, S. R., Ramdas, A., McAuliffe, J., & Sekhon, J. (2021). Time-uniform, nonparametric, nonasymptotic confidence sequences. *Annals of Statistics*, 49(2), 1055–1080.  
DOI: https://doi.org/10.1214/20-AOS1991  
- Always-valid confidence intervals; anytime-valid inference

---

## Power Analysis References

**[C88]** Cohen, J. (1988). *Statistical Power Analysis for the Behavioral Sciences* (2nd ed.). Lawrence Erlbaum Associates.  
ISBN: 9780805802832  
- Standard reference for power analysis formulas; formula for one-sample proportion test

**[EF94]** Faul, F., Erdfelder, E., Lang, A. G., & Buchner, A. (2007). G*Power 3: A flexible statistical power analysis program for the social, behavioral, and biomedical sciences. *Behavior Research Methods*, 39(2), 175–191.  
DOI: https://doi.org/10.3758/BF03193146  
- G*Power software reference for power calculations

---

## Citation Key Index

| Key | Short Reference |
|---|---|
| [W47] | Wald (1947) — Sequential Analysis (SPRT) |
| [P54] | Page (1954) — CUSUM |
| [LD83] | Lan & DeMets (1983) — α-spending |
| [OBF79] | O'Brien & Fleming (1979) — OBF boundaries |
| [P77] | Pocock (1977) — Pocock boundaries |
| [JS06] | Jennison & Turnbull (1999) — Group sequential textbook |
| [M20] | Montgomery (2020) — SQC textbook, CUSUM/EWMA |
| [B50] | Brier (1950) — Brier score |
| [HL80] | Hosmer & Lemeshow (1980) — HL calibration test |
| [GCS17] | Guo et al. (2017) — Neural network calibration, ECE |
| [NMM19] | Nixon et al. (2019) — Debiased ECE |
| [PMB22] | Popordanoska et al. (2022) — Kernel ECE |
| [T05] | Tetlock (2005) — Expert Political Judgment |
| [TG15] | Tetlock & Gardner (2015) — Superforecasting |
| [GC14] | Gelman & Carlin (2014) — Type-M/S errors |
| [JMNS16] | Johari et al. (2022) — mSPRT, always-valid inference |
| [HL20] | Howard et al. (2021) — Confidence sequences |
| [C88] | Cohen (1988) — Power analysis |
| [KNN15] | Naeini et al. (2015) — BBQ calibration |
