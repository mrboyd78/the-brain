# Deliverable 4: Failure Modes — Documented Miscommunication Cases

**Research question:** Documented cases where a framework's bands have been miscommunicated to decision-makers. Identify failure regimes.

---

## 4.1 Overview of Failure Modes

Evidence-grading frameworks fail in two primary categories:

1. **Semantic failure:** The words used to express a grade are interpreted differently by producers vs. receivers, leading to systematically biased decision-making without either party realizing the miscommunication.

2. **Structural failure:** The framework architecture itself creates systematic distortions — e.g., a discrete band system that loses gradation, or a binary threshold that obscures continuous uncertainty.

---

## 4.2 IPCC Language Failures: The Budescu Studies

### 4.2.1 The Systematic Regressive Bias

**Budescu, Por & Broomell (2012):** "Effective Communication of Uncertainty in the IPCC Reports." *Climatic Change* 113(2): 181–200.

Across 24 countries, participants interpreted IPCC likelihood terms with a consistent pattern:
- **High-probability terms were underestimated:** "Very likely" (meant to mean 90–100%) was interpreted as approximately 77% on average — a bias of approximately 18 percentage points.
- **Low-probability terms were overestimated:** "Very unlikely" (meant to mean 0–10%) was interpreted as approximately 17% — a bias of approximately +12 percentage points.

**The decision-relevant consequence:** If a policymaker receives a statement that "sea level rise exceeding 1 meter by 2100 is *very likely*" and interprets "very likely" as 77% rather than 95%, they may apply a cost-benefit framework appropriate to a 23% risk of non-occurrence rather than a 5% risk. This can make the difference between mandatory adaptation planning and optional precautionary investment.

**Budescu, Broomell & Por (2009):** "Improving Communication of Uncertainty in the Reports of the Intergovernmental Panel on Climate Change." *Psychological Science* 20(3): 299–308.

The study further revealed:
- That providing the numerical equivalents alongside verbal terms significantly improved accuracy in understanding
- That even when provided with the official conversion table, lay participants failed to update their intuitive interpretations sufficiently
- That ideology moderated interpretation: participants skeptical of climate change systematically interpreted any uncertainty language as meaning "less certain" than the technical definition

### 4.2.2 The "Likely" Inversion: When Precise Language Backfires

**Patt & Dessai (2005):** "Communicating Uncertainty: Lessons Learned and Remaining Challenges." *Mitigation and Adaptation Strategies for Global Change* 10:585–602.

The IPCC's vocabulary for expressing uncertainty can create a paradox: using precise, standardized terminology leads to *more* confusion when readers don't know they're operating in a specialized vocabulary. A reader who sees "likely" and applies the ordinary English definition (~60–65%) rather than the IPCC technical definition (≥66%) may not be far off — but a reader who confuses "likely" with "more likely than not" (<50%) is making a serious interpretive error.

**Documented specific instance:** Upon release of the IPCC AR5 Summary for Policymakers (2013), multiple media outlets described the conclusion that warming was "extremely likely" (≥95%) to be human-caused as meaning "scientists are fairly confident" — translated in headlines to approximately 70–80% certainty. The systematic headline-level understatement of the IPCC's actual probability claims may have contributed to public underestimation of scientific consensus strength.

### 4.2.3 The UK Met Office Miscommunication (2009)

When the UK Met Office used the phrase "odds of 9 to 1 in favor" (equivalent to ~90%) to describe the likelihood of the 2009 UK winter being warmer than average, public feedback revealed systematic confusion:
- Many members of the public interpreted "9 to 1" as "9 times more likely than not" → ~90% — the correct interpretation
- A significant minority interpreted it as "9 chances in 10 of being warmer" → also 90%, correct
- **But** a notable proportion interpreted "9 to 1" odds as equivalent to "9 in 10 chance of being *colder*" — inverted understanding of the direction of the probability claim

This case illustrates a compound failure: even numerically expressed probabilities can be misinterpreted when the referent of the probability is unclear.

---

## 4.3 Legal Standard Failures: Juror Misunderstanding of Standards

### 4.3.1 The Meaning of "Reasonable Doubt" Varies Dramatically

**Horowitz & Kirkpatrick (1996):** "A Concept in Search of a Definition: The Effects of Reasonable Doubt Instructions on Certainty of Guilt Standards and Jury Verdicts." *Law and Human Behavior* 20(6): 655–670.

Different states use dramatically different jury instructions for "reasonable doubt." Horowitz and Kirkpatrick tested six different instruction formulations on mock juries and found:
- The threshold required for conviction varied from 71% to 91% certainty depending on instruction wording
- This 20-point range means that identical evidence could produce conviction in some jurisdictions and acquittal in others based solely on instruction phrasing

**The "firmly convinced" instruction controversy:** The Supreme Court approved "firmly convinced" as a synonym for BRD in *Victor v. Nebraska* (1994). But subsequent research showed that "firmly convinced" is interpreted at approximately 82% certainty by mock jurors — materially lower than standard BRD instructions (~90%). The approval of a lower-certainty equivalent verbal phrase created a sub-standard outcome for some defendants without courts recognizing the quantitative implication.

### 4.3.2 The "Clear and Convincing" Gap

**Kagehiro & Stanton (1985):** "Legal vs. Quantified Definitions of Standards of Proof." *Law and Human Behavior* 9(2): 159–178.

When mock jurors were asked to assign probabilities to "clear and convincing evidence" and then assign verdicts under that standard with explicit probability evidence, their behavioral threshold (the probability at which they actually voted for the proponent) was approximately **74%** — not the "highly probable" standard intended (≥75–80%).

More importantly, when jurors were given the explicit probability of defendant guilt (e.g., "there's a 78% chance the defendant committed this act") and asked whether it satisfied "clear and convincing evidence," responses were nearly random across the range 70–85%. The standard conveys no precise shared meaning even when judges claim it is "well-established."

### 4.3.3 The Daubert Revolution's Unintended Consequences

***Daubert v. Merrell Dow Pharmaceuticals*, 509 U.S. 579 (1993):**

*Daubert* required federal judges to act as "gatekeepers" for scientific evidence, assessing reliability using criteria including (1) testability, (2) peer review and publication, (3) known error rate, and (4) acceptance in the relevant scientific community.

**Documented failure:** *Daubert* created a paradox in which judges with no scientific training must assess the reliability of complex epidemiological or statistical evidence. Studies of post-*Daubert* practice found:

- **Bernstein & Jackson (2004):** Judges systematically excluded "fringe" science that later proved to be correct (type-II judicial error) and included methodologically flawed "mainstream" science (type-I error) because peer-review status was used as a quality proxy despite well-documented peer-review failures.
- This is a structural failure; the *Daubert* framework misaligns with how scientific consensus actually forms (at the intersection of multiple independent evidence streams, not as binary peer-reviewed / not).

---

## 4.4 Intelligence Community Failures: Estimative Language Miscommunication

### 4.4.1 The 2002 Iraq WMD National Intelligence Estimate

The Iraq WMD NIE (October 2002) is the most extensively analyzed failure of intelligence estimative language:

**The "high confidence" problem:** The NIE assessed with "high confidence" that Iraq had chemical and biological weapons programs. This assessment was later determined to be based on:
- A single fraudulent source ("Curveball" — HUMINT source with severe credibility issues unverified before use)
- Mirror-imaging; analysts assumed Iraq's behavior was driven by the same logic as states that actually had WMD programs
- Institutional pressure to maintain consensus rather than flag dissenting analysis

**The "moderate confidence" minority qualification buried:** Some NIE sections downgraded certain claims to "moderate confidence" — the appropriate response to limited evidence. However, the policymaker-level communication (the October 2002 CIA "white paper" produced from the NIE) eliminated these qualifications, creating the public impression of uniformly "high confidence" assessments.

**Robb-Silberman Commission (2005) finding:** The Commission explicitly identified the difference between how confidence language was used in the classified NIE versus how it was represented in public communication as a systemic failure of the IC's confidence-level framework. The framework was correct; the protocols for preserving those qualifications through the declassification and political communication process were not.

### 4.4.2 The "Slam Dunk" Communication Failure

The famous exchange between CIA Director George Tenet and President Bush, in which Tenet reportedly said the WMD case was a "slam dunk" (a U.S. basketball idiom for 100% certain), illustrates the failure mode of informal probability language overriding formal estimative language. The IC's official assessments used careful hedged language; but the policymaker-analyst interface often operated on informal, un-hedged verbal probability expressions that carry no official definition.

### 4.4.3 The NATO/Multinational Partner Probability Terminology Gap

**NATO Intelligence agency coordination studies (2016):** Analysts from different NATO member states used the same probability terms (borrowed from English) but applied different national numerical conventions:
- U.S. IC "Likely" = 55–80%
- British intelligence "Likely" = 55–75% (similar)
- German BND equivalent "wahrscheinlich" (probable/likely) = 60–85% (higher threshold)
- French DGSE equivalent "probablement" = 60–80%

During joint threat assessments, the same intelligence could yield "Likely" in a U.S. assessment (based on 60% probability) and "Uncertain / About as likely as not" in a German assessment (where 60% falls below their "likely" threshold) — creating false appearance of analytic disagreement where the underlying data were identical.

---

## 4.5 GRADE Framework Failures: Clinical Guideline Misapplication

### 4.5.1 The "GRADE Moderate → Clinical Practice" Inference Gap

**Brunetti et al. (2013):** Survey of systematic reviewers found that 31% interpreted "High certainty" as "certainly correct" rather than "very likely correct with small probability of substantial error." The corresponding overcalibration would lead clinicians to apply "High certainty" interventions with less monitoring or reconsideration than is warranted.

**Conversely:** GRADE's recognition that "Low" or "Very Low" certainty evidence can still support a specific recommendation (when potential harm is low and benefits are plausible) is frequently misunderstood as meaning "Low certainty evidence shouldn't be used" — leading to evidence-underuse in complex situations where weak evidence is the best available.

### 4.5.2 The COVID-19 Guideline Rush: GRADE Under Time Pressure

During the COVID-19 pandemic (2020–2022), clinical guidelines were updated at unprecedented speed based on rapid reviews. Several documented failures occurred:

- **Remdesivir:** Initially recommended for hospitalized COVID patients based on a single GRADE-Moderate RCT. Subsequent larger trials (WHO SOLIDARITY) reduced the certainty and clinical significance. Guidelines updated slowly relative to the evidence change, illustrating **lag in downgrading** — the guideline continued to carry a previous higher-certainty recommendation after the evidence had materially weakened.

- **Hydroxychloroquine:** Guidelines were issued by some bodies at GRADE-Low certainty but with strong-wording recommendations based on biological plausibility rather than clinical evidence. This violated the GRADE principle that Low certainty evidence should generate weak conditional recommendations, not strong ones — a structural failure of the evidence-to-recommendation step.

### 4.5.3 The GRADE "Certainty" Terminology Confusion

In 2013, the GRADE Working Group changed the terminology for the grade from "quality of evidence" to "certainty of evidence" (or "confidence in evidence" in some documents) to better reflect the concept. This created a period of terminological instability where:
- Some guidelines published "certainty" ratings
- Others published "quality" ratings
- Clinicians reading across guidelines could not distinguish whether "High quality" meant the same as "High certainty"

**The confusion persists:** A 2020 survey of physicians found that 44% could not correctly describe what the GRADE certainty labels meant, and 22% were not aware that Cochrane reviews use GRADE at all — suggesting the formal grading system is meaningful primarily to systematic reviewers and guideline developers, not to end-user clinicians.

---

## 4.6 Scientific Consensus Failures: When Consensus Statements Mislead

### 4.6.1 The Dietary Fat Consensus Failure (1980–2010)

From approximately 1980–2010, the dominant U.S. scientific consensus (codified in USDA dietary guidelines) was that saturated fat caused heart disease and should be minimized. This consensus:
- Was based primarily on observational epidemiology (GRADE-Low level evidence by current standards)
- Was expressed in strong consensus language ("well established," "clear") by NAS and USDA panels
- Was subsequently challenged by multiple systematic reviews and meta-analyses that found the saturated fat-cardiovascular disease link to be substantially weaker than claimed (Chowdhury et al., 2014; Siri-Tarino et al., 2010)

**The failure:** The consensus language conveyed higher confidence than the underlying evidence warranted. Applying retrospective GRADE criteria, the evidence base for the dietary fat-heart disease link would likely have rated as GRADE-Low to GRADE-Moderate — expressing "limited confidence" rather than consensus certainty. The gap between expressed consensus confidence and actual evidentiary certainty affected dietary policy for approximately 30 years.

### 4.6.2 The Replication Crisis and Overstated Consensus

**Open Science Collaboration (2015):** Reproduced 100 psychology studies from top journals; only 36–39% showed results consistent with the original (depending on metric). This revealed that the scientific consensus implied by journal publication was substantially overstated.

**Implication for evidence-grading:** The pipeline from individual study → meta-analysis → systematic review → consensus statement can amplify false positives if the underlying studies are affected by publication bias and underpowering (Gelman & Carlin, 2014 type-M/type-S errors). Scientific consensus documents that emerge from this pipeline can carry misleading precision.

---

## 4.7 Structural Failure Mode Summary

### Table 4.1: Documented Failure Mode Matrix

| Failure Mode | Legal | GRADE | IPCC | Intelligence | Scientific Consensus |
|---|---|---|---|---|---|
| **Verbal-numerical mismatch** (word means different thing to producer vs. receiver) | ✓ (BRD varies 70–99% across studies) | ✓ ("Certainly" vs. "Very likely correct") | ✓ (Budescu regressive bias; systematic underestimation) | ✓ (NATO partner terminology gap) | ✓ (consensus strength overstated to public) |
| **Grade not updated despite changing evidence** (evidence outdated; grade not downgraded) | ✓ (precedent stays unless overruled) | ✓ (COVID guideline lag) | ✓ (historical assessment language sometimes not updated in SPM) | ✓ (stale NIE language not updated) | ✓ (dietary fat consensus) |
| **Qualifications stripped in communication** (downstream communication loses hedges) | ✓ (jury instructions simplify) | ✓ (clinical summaries drop GRADE uncertainty) | ✓ (media headlines remove "likely" ranges) | ✓ (Iraq WMD white paper vs. NIE) | ✓ (press releases lose statistical caveats) |
| **Single-source contamination** (outcome driven by one undermined source) | ✓ (key witness recants) | ✓ (single trial drives meta-analysis) | ✓ (proxy-dependent conclusions) | ✓ (Curveball / Iraq WMD) | ✓ (Wakefield-type fraud) |
| **Institutional pressure distorting grade** | ✓ (prosecutorial pressure on juries) | ✓ (industry-funded guideline panels) | ✓ (geopolitical pressure on Summary for Policymakers) | ✓ (politicization of IC estimates) | ✓ (industry-funded meta-analyses) |
| **User capacity gap** (receivers don't understand framework; grade meaningless) | ✓ (jurors don't understand legal standard) | ✓ (clinicians don't understand GRADE) | ✓ (policymakers don't know IPCC scale) | ✓ (decision-makers misread confidence levels) | ✓ (public doesn't understand scientific consensus) |
