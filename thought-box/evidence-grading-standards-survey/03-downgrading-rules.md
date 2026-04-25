# Deliverable 3: Downgrading Rules — Per-Framework Conditions and Cross-Framework Analysis

**Research question:** For each framework, list the conditions that lower the grade. Identify downgrading conditions that appear in multiple frameworks.

---

## 3.1 The Concept of Downgrading

"Downgrading" refers to a systematic process of reducing the expressed confidence level from what would be indicated by the nominal evidence type. Each framework has a set of conditions — methodological, logical, or evidentiary — that trigger a reduction in confidence expression. These mechanisms reveal what each framework considers the primary threats to epistemic reliability.

---

## 3.2 Legal Standards: Conditions That Affect the Standard Applied

The legal system does not "downgrade" within a standard once it is determined — rather, it selects the applicable standard based on the legal proceeding type, and the quality of evidence affects the *weight* given to individual items rather than the standard itself. However, the following conditions affect the effective evidentiary burden a party must meet:

### 3.2.1 Framework-Specified Conditions Reducing Evidence Weight

| Condition | Effect | Legal Doctrine |
|---|---|---|
| **Hearsay** | Excludes evidence entirely (unless excepted) | FRE 801-807; *Crawford v. Washington*, 541 U.S. 36 (2004) |
| **Chain of custody deficiency** | Reduces or eliminates weight; can result in exclusion | Authentication doctrine (FRE 901) |
| **Witness bias / interest** | Court instructs jury to weight testimony critically | Impeachment doctrine (FRE 607-609) |
| **Expert opinion beyond competence** | Excluded or limited to reliable methods (*Daubert*) | *Daubert v. Merrell Dow*, 509 U.S. 579 (1993); FRE 702 |
| **Circumstantial nature of evidence** | Does not lower standard but affects sufficiency analysis | Jackson v. Virginia standard for sufficiency |
| **Identification reliability** | Court may apply heightened scrutiny | Eyewitness reliability (*Manson v. Brathwaite*, 432 U.S. 98 (1977)) |
| **Confession obtained involuntarily** | Excluded from jury consideration | *Miranda v. Arizona*, 384 U.S. 436 (1966) |
| **Prior criminal record (bad acts)** | Generally inadmissible for propensity; narrow exceptions | FRE 404(b) |

**Legal analogue to "upgrading":** Courts can *raise* the effective evidentiary threshold in specific circumstances — e.g., *Addington v. Texas* held that civil commitment requires clear and convincing evidence because the high stakes of deprivation of liberty justify a higher standard than preponderance. This standard-selection based on stakes is the closest legal analogue to domain-specific calibration of confidence thresholds.

### 3.2.2 Conditions Reducing the Applicable Standard (Cross-Standard)

The relevant legal domain determines which standard applies:

| Domain Type | Applicable Standard | Rationale |
|---|---|---|
| Most civil litigation | Preponderance | Private parties; monetary remedy |
| Fraud, civil rights violations | Clear and convincing | Severity; potential to chill legitimate behavior |
| Civil commitment | Clear and convincing (*Addington*) | Liberty deprivation |
| Termination of parental rights | Clear and convincing (*Santosky v. Kramer*, 1982) | Fundamental right affected |
| Criminal prosecution | Beyond a reasonable doubt (*Winship*) | Criminal penalty; social stigma |
| Death penalty | BRD + heightened mitigation review | Irreversibility |

---

## 3.3 GRADE Framework: Five Downgrading Domains

Source: Guyatt et al. (2011), "GRADE guidelines: 1. Introduction—GRADE evidence profiles and summary of findings tables," *Journal of Clinical Epidemiology* 64(4):383-394; and the full GRADE series.

GRADE's downgrading system is the most formalized and explicitly codified of all five frameworks. Each domain can trigger a -1 (serious concern) or -2 (very serious concern) downgrade from the starting evidence level.

### Table 3.1: GRADE Downgrading Domains

| Domain | Trigger Conditions | Magnitude | Decision Criteria |
|---|---|---|---|
| **1. Risk of Bias** (Study Limitations) | Lack of concealed randomization; absence of blinding; >20% loss to follow-up; selective outcome reporting; stopped early for benefit | -1 or -2 | Based on cumulative judgment across included studies |
| **2. Inconsistency** | Wide variation in point estimates; non-overlapping confidence intervals; heterogeneity I² >75%; evidence of subgroup effects unexplained | -1 or -2 | Magnitude and plausibility of unexplained heterogeneity |
| **3. Indirectness** | Different populations; different interventions/comparators; surrogate outcomes; indirect comparisons (network meta-analysis without direct data) | -1 or -2 | How different the available evidence is from the ideal evidence for the decision |
| **4. Imprecision** | Wide confidence intervals crossing important thresholds (<400 events for binary outcomes; <200 per arm as a heuristic); insufficient optimal information size (OIS) | -1 or -2 | Whether CIs cross minimal important difference thresholds |
| **5. Publication Bias** | Small-study effects; funnel plot asymmetry; many small studies with positive results and few large studies; lack of trial registration | -1 | Strength of suspicion; difficult to downgrade 2 levels for this domain alone |

### Table 3.2: GRADE Upgrading Domains (for observational studies starting at Low)

| Domain | Trigger Condition | Magnitude |
|---|---|---|
| **Large effect** | RR or OR ≥2 or ≤0.5 without confounders explaining it | +1 |
| **Very large effect** | RR or OR ≥5 or ≤0.2 without confounders | +2 |
| **Dose-response gradient** | Clear gradient of effect with dose | +1 |
| **All plausible confounders go opposite direction** | Residual confounding would reduce the observed effect | +1 |

### 3.3.1 GRADE Downgrading Decision Flow

```
Starting point:
  RCTs → High (4 circles)
  Observational → Low (2 circles)

For each of 5 domains:
  Serious concern → -1
  Very serious concern → -2

Minimum possible: Very Low (cannot go below 1 circle)
Maximum possible downgrade: -2 per domain × 5 domains = -10 
                             (but floor is at Very Low)
```

**Practical constraint:** In practice, reviewers rarely downgrade all five domains simultaneously. The most common downgrading pattern is Risk of Bias + Imprecision (two domains), dropping an RCT from High to Moderate or Low.

---

## 3.4 IPCC Uncertainty Language: Conditions Reducing Confidence

Source: Mastrandrea et al. (2010); Mach et al. (2017), "Scientific Achievements and Uncertainties," *Nature Climate Change*.

### 3.4.1 Evidence Conditions That Reduce IPCC Confidence Level (Scale B)

The IPCC Confidence scale is explicitly two-dimensional: **strength of evidence × degree of agreement**. Downgrading on either dimension reduces the overall confidence level.

| Condition Category | Specific Trigger | Direction | Dimension Affected |
|---|---|---|---|
| **Limited observations** | Few systematic measurements; sparse spatial/temporal coverage | ↓ confidence | Evidence strength |
| **Physically incomplete models** | Climate models unable to represent key physical processes at required resolution | ↓ confidence | Evidence strength |
| **Conflicting observational evidence** | Different observational datasets yield substantially different estimates | ↓ confidence | Evidence strength AND agreement |
| **Expert disagreement** | Significant minority of expert authors hold systematically different interpretations | ↓ confidence | Agreement |
| **Limited mechanistic understanding** | Physical process is observed but not mechanistically explained | ↓ confidence | Evidence strength |
| **History of model error** | Process has been systematically misrepresented in past assessments | ↓ confidence | Evidence strength |
| **Short observational record** | Trend-detection limited by record length | ↓ confidence | Evidence strength |
| **Non-stationarity concerns** | Future conditions may be outside the range of past observations | ↓ confidence | Evidence strength |

### 3.4.2 Evidence Conditions That Reduce IPCC Likelihood Estimate (Scale A)

| Condition | Effect on Likelihood Expression |
|---|---|
| Structural uncertainty in models | Widen the likelihood range; use "more likely than not" instead of "likely" |
| Scenario uncertainty | Conditional probability expression ("likely under RCP8.5; about as likely as not under RCP2.6") |
| Low-probability / high-impact tails | May add explicit low-probability catastrophic tail statements alongside main likelihood |
| Initial condition uncertainty | Acknowledged by ensemble approach; widens stated probability range |

### 3.4.3 The IPCC Cross-Chapter Box Framework

AR6 introduced formal Cross-Chapter Boxes on "confidence" that required lead authors to document the evidence table explicitly:

```
Evidence type      | Abundant / Medium / Limited / No information
Evidence quality   | Strong / Moderate / Ambiguous / Contradictory
Degree of agreement| High / Medium / Low
```

These three inputs are combined to produce the final Confidence level using a structured matrix.

---

## 3.5 Intelligence Community Estimative Language: Conditions Reducing Confidence

Source: ICD 203 (2015), *Analytic Standards*; *Intelligence Community Directive 206, Sourcing Standards*; Pherson & Heuer (2012).

### 3.5.1 Conditions Reducing IC Probability Estimates

| Condition | Effect on Probability Language | Standard Reference |
|---|---|---|
| **Unique-sourcing** (single source for key claim) | Analysts must caveat; often shifts from "probably" to "possibly" | ICD 206 sourcing standards |
| **Source with known access limitations** | Probability language weakened; confidence marker lowered | ICD 203 §7 |
| **Contradicting evidence from other sources** | Reduces probability toward "even chance" or explicitly acknowledged uncertainty | Structured Analytic Technique guidance |
| **Short track record of source** | Confidence level reduced; probability shifted toward midpoint | Source validation standards |
| **Known denial and deception (D&D)** | Can *reverse* probability direction; "likely" shifts to "possibly" or lower | CI/OPSEC considerations |
| **Mirror-imaging** (assuming adversary thinks like us) | Recognized analytic bias; corrects toward lower probability | ICD 203 analytic bias standards |
| **Absence of evidence** | Not treated as "evidence of absence" — does not lower probability mechanically but constrains reasoning | ICD 203 logical standards |

### 3.5.2 Conditions Reducing IC Confidence Levels

| Condition | From → To | Rationale |
|---|---|---|
| Single source | High → Moderate or Low | Insufficient corroboration |
| Sources not independently corroborated | High → Moderate | Possible single-source contamination |
| Evidence collected >2 years ago | Moderate → Low | Information currency concern |
| Highly technical subject outside analyst expertise | High → Moderate | Analytic competence boundary |
| Question requires prediction of unprecedented event | Any → One level lower | Inherent forecasting uncertainty |
| Political/institutional pressure suspected | Flag, not downgrade | Integrity protection mechanism |

### 3.5.3 The IC "Linchpin" Framework

ICD 203 introduces the concept of "linchpin assumptions" — foundational premises upon which an analytic judgment rests. If a linchpin assumption is:
- Unverified but plausible: Probability stays, confidence drops
- Directly contested by evidence: Probability shifts downward
- Known to be false: Assessment is revised entirely

---

## 3.6 Scientific Consensus: Conditions Reducing Consensus Characterization

| Condition | Effect | Examples |
|---|---|---|
| **Replication failure** | Emerging consensus → Contested | Many nutrition studies; psychology replication crisis |
| **Methodological problems discovered post-hoc** | Established → Contested status | Wakefield autism-vaccine study; early fibromyalgia-drug trials |
| **Publication bias exposure** | Consensus weakened; effect size revision | Antidepressant selective reporting; Turner et al. 2008 |
| **Novel heterodox evidence** | May begin consensus shift | Helicobacter pylori (Marshall & Warren initially contested) |
| **Re-analysis of primary data** | Consensus revised downward if effect disappears | Preclinical to clinical translation failures |
| **Expert panel composition bias** | Advisory body credibility reduced | Conflict of interest concerns in guideline panels |

---

## 3.7 Cross-Framework Downgrading Conditions

### Table 3.3: Downgrading Conditions Present in Multiple Frameworks

| Downgrading Condition | Legal | GRADE | IPCC | IC / Intelligence | Science Consensus |
|---|---|---|---|---|---|
| **Single/unique sourcing** | ✓ (single-witness credibility) | ✓ (single study; no replication) | ✓ (limited observations) | ✓ (explicit single-source rule) | ✓ (unreplicated finding) |
| **Bias / conflicting interest of source** | ✓ (witness bias; expert for hire) | ✓ (risk of bias domain) | ✓ (acknowledged for some expert elicitation) | ✓ (source motivation analysis) | ✓ (COI disclosure requirements) |
| **Inconsistency across evidence** | ✓ (contradictions in testimony) | ✓ (inconsistency domain — formally defined) | ✓ (conflicting evidence reduces confidence) | ✓ (contradicting reports reduce probability) | ✓ (replication failure) |
| **Indirectness / relevance gap** | ✓ (relevance determinations; FRE 402-403) | ✓ (indirectness domain — formally defined) | ✓ (indirect vs. direct observations) | ✓ (HUMINT vs. technical intelligence gap) | ✓ (laboratory vs. real-world generalizability) |
| **Imprecision / measurement uncertainty** | ✓ (scientific testimony standards; *Daubert*) | ✓ (imprecision domain — formally defined) | ✓ (model uncertainty; observational precision) | ✓ (MASINT vs. HUMINT precision differences) | ✓ (statistical uncertainty; effect size precision) |
| **Publication/reporting bias** | ✓ (selective evidence presentation) | ✓ (publication bias domain — formally defined) | ✓ (acknowledged; addressed via all-source assessment) | ✓ (availability bias in reporting chain) | ✓ (file drawer problem) |
| **Currency / timeliness** | ✓ (stale evidence impeachability) | ✓ (older studies get more scrutiny) | ✓ (observational record length) | ✓ (explicit currency criteria in ICD 206) | ✓ (recent vs. older evidence weighting) |
| **Denial, deception, or adversarial manipulation** | ✓ (fraud, perjury; FRE 616) | ✓ (selective reporting; fraud) | — (not formally applicable) | ✓ (explicit D&D protocols) | ✓ (fraud detection; post-publication review) |

**Key finding:** Six out of eight cross-framework downgrading conditions appear in all five frameworks (legal, GRADE, IPCC, IC, science). The two conditions appearing in fewer frameworks are:
- **Denial and deception / adversarial manipulation:** Most relevant to intelligence (dedicated protocols) and law (perjury doctrine), but absent from IPCC (which does not model adversarial actors) and rare in medical research context (though fraud exists)
- **Currency/timeliness:** Most formalized in intelligence (specific date-based currency rules in ICD 206) and partially in GRADE (recency preferences); less formalized in law and scientific consensus

### 3.7.1 The Universal Downgrading Foundation

The convergence on six common downgrading conditions suggests that **all evidence-grading frameworks are solving the same underlying epistemic problem**: distinguishing high-quality, relevant, consistent evidence from low-quality, biased, inconsistent, or outdated evidence. The terminology differs; the structure is isomorphic.

This isomorphism is the empirical basis for the unified scale proposed in Deliverable 5.

### 3.7.2 Unique Downgrading Conditions Per Framework

| Framework | Unique Downgrading Condition | Not Present in Others |
|---|---|---|
| Legal | **Hearsay exclusion rule** — automatic bar on out-of-court statements for truth (FRE 801) regardless of reliability | No other framework categorically excludes evidence categories by procedural type |
| Legal | **Constitutional violations** (4th/5th/6th Amendment) trigger exclusionary rule regardless of probative value | Evidence excluded for policy reasons unrelated to epistemic quality — unique to law |
| GRADE | **Study design hierarchy starting point** (RCTs start High; observational start Low) | Other frameworks don't formally devalue observational evidence at the outset |
| GRADE | **Optimal Information Size (OIS)** threshold — a power-analytical criterion for imprecision | Quantitative power threshold as downgrading trigger is unique to GRADE |
| IPCC | **Scenario uncertainty** (sensitivity of conclusion to emissions pathway assumptions) | Other frameworks don't account for contingent future-state dependency |
| IC | **Denial and Deception (D&D)** analysis protocols | Formal adversarial intent accounting is unique to intelligence contexts |
| IC | **Source access validation** (did this source have physical access to the claimed information?) | Other frameworks assume evidence is produced in good faith |
| Science | **Clinical-to-bench translation failure** (animal/lab → human generalizability) | Specific translational validity concerns arise primarily in biomedical science |
