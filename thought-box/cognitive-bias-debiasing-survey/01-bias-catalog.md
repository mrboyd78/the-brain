# Deliverable 1: Categorized Bias Catalog with Field-Condition Effect Sizes

> **Survey**: Cognitive Biases and Debiasing Interventions  
> **Scope**: Kahneman & Tversky; Gilovich, Griffin & Kahneman (2002); Kahneman (2011)  
> **Date Compiled**: April 2026

---

## 1.1 Framework: Dual-Process Theory

All major cognitive biases are understood within the dual-process framework (Kahneman, 2011; Stanovich & West, 2000):

- **System 1** (automatic, fast, associative): generates intuitive judgments, heuristics, and emotional responses without deliberate effort. Source of most documented biases.
- **System 2** (deliberate, slow, analytical): can override System 1 outputs but is cognitively expensive, easily depleted, and frequently rationalizes rather than corrects System 1 outputs.

The **bias catalog** below is organized by the family of cognitive mechanism producing the bias. For each entry, the strongest field-condition (not exclusively laboratory) evidence is cited alongside the best available effect size estimate.

---

## 1.2 Category I: Judgment Under Uncertainty (Heuristics)

### 1.2.1 Anchoring and Insufficient Adjustment

**Definition**: The tendency to rely excessively on the first piece of numerical information encountered (the "anchor") when making estimates, and to adjust insufficiently from it.

**Original evidence**: Tversky & Kahneman (1974) — spinning a wheel of fortune in front of subjects then asking them to estimate African countries in the UN produced an anchor effect: high-wheel subjects guessed 45%, low-wheel subjects guessed 25%.

**Field-condition evidence**:
- **Legal sentencing**: Englich, Mussweiler & Strack (2006, *Personality and Social Psychology Bulletin*): Prosecutors' arbitrary sentencing demands shifted judges' actual sentences. Judges who rolled a high number on a die before sentencing gave longer sentences. *d* ≈ 0.89.
- **Real estate**: Northcraft & Neale (1987, *Organizational Behavior and Human Decision Processes*): Real estate agents' property valuations were anchored by an arbitrary listing price. Expert agents were as biased as novices. Anchor effect: ~12% shift in valuation per $10,000 anchor unit.
- **Medical diagnosis**: Saposnik et al. (2016, *European Journal of Neurology*): Anchoring to initial diagnosis in stroke patients delayed correct re-diagnosis. Prevalence estimated at 40–48% of physician errors.
- **Salary negotiations**: Chapman & Johnson (1994, *Organizational Behavior and Human Decision Processes*) — anchors affected salary estimates in HR professionals by 12–15%.

**Field-condition effect size**: *d* ≈ 0.50–0.90 depending on domain; salary/legal domains show larger effects than pure numerical estimation tasks.

**⚠️ Replication note**: Fudenberg, Levine & Maniadis (2012) found that with real monetary stakes, anchoring effects for coherent objects (prices subjects had considered) were robust; for novel random anchors, effects were smaller in high-incentive conditions.

---

### 1.2.2 Availability Heuristic

**Definition**: Probability and frequency are judged by the ease with which instances come to mind, producing systematic errors when memorable events are not proportionally frequent.

**Original evidence**: Tversky & Kahneman (1973) — people estimated "words beginning with K" as more common than "words with K as 3rd letter" despite the reverse being true.

**Field-condition evidence**:
- **Insurance markets**: Kunreuther et al. (1978, *Disaster Insurance Protection*): Earthquake and flood insurance purchases spike dramatically after disasters (increased availability) then fall over subsequent years, independent of underlying risk changes.
- **Medical risk assessment**: Schwartz, Woloshin & Welch (2005, *Annals of Internal Medicine*): Physicians systematically overestimated rare, memorable disease risks and underestimated common, less salient risks. Cancer risk overestimation by 2–3× observed.
- **Terrorism risk**: Sunstein (2003, *Journal of Human Rights*): Post-9/11 flight reductions led to estimated 1,595 additional highway deaths in the U.S. (Gigerenzer, 2004) — a measurable field consequence of availability-driven risk reassessment.
- **Investor behavior**: Barber & Odean (2008, *Review of Financial Studies*): Retail investors were disproportionately drawn toward stocks appearing in news (high availability) regardless of underlying fundamentals.

**Effect size**: Difficult to compute a single *d*; behavioral consequences are measured in percentage miscalibration. Stock attention bias produced ~3–4% excess returns in the subsequent month for salient stocks, reversing within 12 months.

---

### 1.2.3 Representativeness Heuristic

**Definition**: Probability is judged by how well an event matches a prototype or stereotype, ignoring base rates.

**Original evidence**: Kahneman & Tversky (1973) — "Linda the feminist bank teller" conjunction fallacy; Tversky & Kahneman (1983) — formal demonstration that P(A∩B) > P(A) in representativeness-driven judgments.

**Field-condition evidence**:
- **Medical diagnosis**: Eddy (1982, *JAMA*): Physicians ignored base rates in interpreting diagnostic test results — a high-specificity test for a rare disease was interpreted as nearly certain given a positive result (~79% of physicians misinterpreted); Bayesian correct answer: ~7.8% positive predictive value.
- **Financial analysis**: De Bondt & Thaler (1985, *Journal of Finance*): Stocks categorized as "glamour" (representative of successful companies) were systematically overpriced relative to "value" stocks; 3–5-year reversal consistent with representativeness-driven overreaction. *d* ≈ 0.60 on 5-year returns.
- **Personnel selection**: Highhouse (2008, *Perspectives on Psychological Science*): Clinical judgment (representativeness-based) consistently underperformed actuarial models across 100+ studies comparing the two approaches.

**Notable moderator**: When base rate information is made explicit and salient, representativeness bias is reduced (Ajzen, 1977) — but the reduction is partial, not complete.

---

### 1.2.4 Overconfidence and Miscalibration

**Definition**: Three related phenomena — (a) **overprecision**: confidence intervals are too narrow; (b) **overplacement**: belief that one is better than average; (c) **overestimation**: inflating one's own score or probability of success.

**Original evidence**: Lichtenstein, Fischhoff & Phillips (1982) — almanac question confidence intervals; 90% confidence intervals contained the correct answer only ~50% of the time.

**Field-condition evidence**:
- **Forecasting**: Tetlock (1999–2003, *Expert Political Judgment*, 2005): 284 experts, 27,450 predictions over 20 years. Experts were roughly as accurate as a chimp throwing darts. Calibration scores were poor across domains; "fox" thinkers (diverse sources) outperformed "hedgehog" thinkers (single-theory) on calibration.
- **Financial markets**: Barber & Odean (2001, *Journal of Finance*): Overconfident traders traded 45% more frequently than others; trading reduced net returns by 3.7% annually versus passive buy-and-hold.
- **Medical prognosis**: Christensen-Szalanski & Bushyhead (1981, *Cognitive Psychology*): Physicians' stated probabilities of pneumonia correlated poorly with actual X-ray confirmation rates; calibration *d* ≈ 0.45 vs. perfectly calibrated baseline.
- **Construction projects**: Flyvbjerg (2003, *Oxford Review of Economic Policy*): Cost overruns averaged 45% for road projects, 50% for rail, 28% for bridge/tunnel in a dataset of 258 megaprojects. Systematic optimism bias (planning fallacy) consistently across 20-year dataset.

**Planning fallacy specifically** (Kahneman & Tversky, 1979): Mean overrun in field studies is 25–45% for professional projects, consistent across sectors and continents (Flyvbjerg, 2003, 2014).

---

### 1.2.5 Hindsight Bias

**Definition**: After learning an outcome, individuals systematically overestimate the probability they would have assigned to that outcome beforehand ("I knew it all along").

**Original evidence**: Fischhoff (1975) — subjects who knew Nixon's trip outcomes rated those outcomes as more inevitable than those who did not.

**Field-condition evidence**:
- **Legal/medical judgments**: Kamin & Rachlinski (1995, *Law and Human Behavior*): Mock jurors who knew a flood caused property damage rated the defendant's negligence higher than identical juries not told the outcome. *d* ≈ 0.67.
- **Medical error review**: Caplan, Posner & Cheney (1991, *JAMA*): Physicians reviewing anesthesia cases rated care as more substandard when told the patient died vs. survived identical case descriptions. *d* ≈ 0.55.
- **Investment**: Fischhoff & Beyth (1975) — Israeli–Arab War predictions; post-outcome recall of pre-war confidence levels inflated by 20–30 percentage points.

**Field effect size**: *d* ≈ 0.50–0.70 across legal, medical, and political domains.

---

## 1.3 Category II: Social and Self-Related Biases

### 1.3.1 Confirmation Bias

**Definition**: The tendency to search for, interpret, and recall information in a way that confirms pre-existing beliefs; asymmetric updating — disconfirming evidence is discounted or avoided.

**Original evidence**: Wason (1960) — 2-4-6 rule discovery task; 79% of subjects failed to test disconfirming hypotheses.

**Field-condition evidence**:
- **Intelligence analysis**: Jervis (2010, *Why Intelligence Fails*): Pre-war Iraq WMD analysis showed systematic confirmation bias — analysts sought information consistent with the prevailing hypothesis and failed to weight disconfirming signals. The Iraq Survey Group later found no WMD program.
- **Medical diagnosis**: Wolf, Gruppen & Billi (1985, *Journal of Medical Education*): Physicians who had formed an initial diagnosis were significantly less likely to order tests that might disconfirm it. Hypothesis confirmation rate ~65% vs. ~35% hypothesis disconfirmation seeking.
- **Criminal justice**: Charman (2013, *Psychological Science*): Once a suspect was identified, detectives selectively attended to confirming evidence. In mock crime scenarios, 40% of confirmed suspects were innocent; conviction-consistent evidence interpretation remained even with exculpatory evidence.
- **Investment analysis**: Friesen & Weller (2006, *Journal of Economic Behavior & Organization*): Professional investor reports showed disproportionate citation of supporting evidence; buy recommendations contained 3× more positive mentions than negative.

**Field effect size**: *d* ≈ 0.60–0.80 in hypothesis testing tasks; behavioral consequences difficult to size, but measurable in wrongful convictions, intelligence failures, and investment performance.

---

### 1.3.2 Fundamental Attribution Error (FAE) / Correspondence Bias

**Definition**: Tendency to over-attribute others' behavior to their character and under-weight situational factors; the opposite error is less common for oneself (actor-observer asymmetry).

**Original evidence**: Jones & Harris (1967) — subjects attributed pro-Castro essays to genuine beliefs even when told the author was assigned the position.

**Field-condition evidence**:
- **Organizational evaluations**: Fast et al. (2012, *Organizational Behavior and Human Decision Processes*): Managers consistently over-attributed employee under-performance to personal traits rather than structural constraints. *d* ≈ 0.45.
- **Consumer behavior**: Langendijk & van Kleef (2017, *Food Quality and Preference*): Health judgments of people eating junk food rated as reflecting character rather than context (cafeteria availability, stress). Dispositional attribution dominant in 73% of scenarios.
- **Intergroup conflict**: Ross & Ward (1996, *Nebraska Symposium on Motivation*): Attributional asymmetry (own group behavioral; out-group dispositional) is robust in international conflict contexts.

---

### 1.3.3 Bias Blind Spot

**Definition**: People accurately identify biases in others but fail to recognize the same biases in themselves.

**Original evidence**: Pronin, Lin & Ross (2002, *Personality and Social Psychology Bulletin*) — subjects rated themselves as less susceptible to 37 cognitive biases than peers.

**Field-condition evidence**:
- **Medical professionals**: Blumenthal-Barby & Krieger (2015, *American Journal of Bioethics*): Physicians acknowledged that peers had cognitive biases affecting clinical judgment; fewer than 20% acknowledged personal susceptibility.
- **Professionals after bias training**: West et al. (2012, *Journal of Personality and Social Psychology*): Higher cognitive sophistication (SAT scores, thinking-style measures) did NOT reduce bias blind spot; in some analyses, more sophisticated thinkers showed larger blind spots.

**Critical implication**: The bias blind spot is a direct obstacle to debiasing — it means people who most need debiasing are least likely to believe they do.

---

### 1.3.4 Sunk Cost Fallacy / Escalation of Commitment

**Definition**: Continued investment in a failing course of action due to prior investment rather than expected future returns.

**Original evidence**: Arkes & Blumer (1985, *Organizational Behavior and Human Decision Processes*) — classic ski trip experiment.

**Field-condition evidence**:
- **Military operations**: Moon (2001, *Journal of Applied Psychology*): Escalation of commitment in military simulations — commanders continued failing operations at rate ~35% higher than normatively optimal when they had initiated the operation vs. inherited it.
- **Business investment**: Staw (1981, *Organizational Behavior and Human Decision Processes*): Financial resource allocation increased by 40% toward failing units when the prior decision-maker was also the evaluator. *d* ≈ 0.65.
- **Software development**: Keil & Robey (1999, *Journal of MIS*): IT project escalation: 16–32% of major IT projects continued past rational stopping points due to sunk cost logic.

---

## 1.4 Category III: Probability and Risk Biases

### 1.4.1 Base Rate Neglect

**Definition**: Ignoring the prior probability of events in favor of specific case-based information (related to representativeness).

**Field-condition evidence** (see 1.2.3 Medical diagnosis): Eddy (1982) medical case; also:
- **Screening tests**: Gigerenzer & Hoffrage (1995, *Psychological Review*): 95% of medical textbook examples of test interpretation contained base rate neglect errors. When reformatted as frequency statements (natural frequencies), error rates dropped from ~72% to ~24%.

---

### 1.4.2 Neglect of Probability (Denominator Neglect)

**Definition**: Judgments of risk focus on the numerator (number of bad events) rather than the denominator (base population); vivid single cases dominate over statistics.

**Field-condition evidence**:
- **Medical risk communication**: Woloshin, Schwartz & Welch (2008, *Annals of Internal Medicine*): Patients rated a medication significantly riskier when told "10 in 1,000 patients experience a serious side effect" vs. "1%," even though these are identical.
- **Policy**: Slovic, Finucane & MacGregor (2007): Nuclear energy risk perception far exceeds statistical mortality risk; airplane crash perception exceeds relative frequency of automobile fatality.

---

### 1.4.3 Loss Aversion

**Definition**: Losses loom approximately 2× as large as equivalent gains in utility (λ ≈ 2.0–2.5 in prospect theory).

**Original evidence**: Kahneman & Tversky (1979, *Econometrica*) — prospect theory derivation.

**Field-condition evidence**:
- **Labor markets**: Camerer et al. (1997, *Quarterly Journal of Economics*): New York City cab drivers quit earlier on high-demand days (when wages were high) and worked longer on low-demand days — consistent with daily income targets and loss aversion to falling short. *d* ≈ 0.45.
- **Real estate**: Genesove & Mayer (2001, *Quarterly Journal of Economics*): Sellers who would realize a nominal loss set significantly higher listing prices and kept properties on the market longer. Loss aversion coefficient estimated at λ ≈ 2–3 in this naturalistic real estate dataset.
- **Sports**: Pope & Schweitzer (2011, *American Economic Review*): PGA Tour golfers putted measurably better for par (loss frame) than for birdie (equivalent gain) — *d* ≈ 0.35; across 2.5 million putts.

---

## 1.5 Category IV: Memory and Judgment Biases

### 1.5.1 Framing Effects

**Definition**: Logically equivalent information presented differently produces systematically different choices.

**Original evidence**: Tversky & Kahneman (1981) — Asian disease problem; "save 200 lives" vs. "400 people die" framing reversed preferences in 78% of participants.

**Field-condition evidence**:
- **Medical treatment decisions**: McNeil et al. (1982, *New England Journal of Medicine*): Lung cancer treatment preferences (surgery vs. radiation) reversed depending on survival vs. mortality frame. Frame reversal occurred in 44% of physicians, 72% of patients.
- **Consumer behavior**: Gärling et al. (2006, *Journal of Economic Psychology*): Credit card surcharges vs. cash discounts: behaviorally identical but produced 15–25% difference in adoption rates.

---

### 1.5.2 Status Quo Bias / Default Effects

**Definition**: Preference for the current state; departures from the default are perceived as losses.

**Field-condition evidence** (one of the largest effect sizes in behavioral economics):
- **Organ donation**: Johnson & Goldstein (2003, *Science*): Opt-in countries (Germany, UK): 12–17% donation consent rates; opt-out countries (France, Austria): 85–99% consent rates. The single largest behavioral intervention effect in published public policy research.
- **Retirement savings**: Madrian & Shea (2001, *Quarterly Journal of Economics*): Auto-enrollment in 401(k) plans increased participation from 49% to 86% — a 37 percentage point difference with no change in incentives.

---

### 1.5.3 Planning Fallacy (Optimism Bias)

**Definition**: Underestimation of time, cost, and risk of future actions and overestimation of benefits (related to overconfidence).

**Field-condition evidence** (most robust in naturalistic conditions):
- **Capital projects**: Flyvbjerg, Holm & Buhl (2002, *Journal of the American Planning Association*): 258 infrastructure projects over 70 years across 20 countries. Average cost overrun: 28% (bridges/tunnels), 45% (roads), 50% (rail). Overruns were consistent regardless of project age, geography, or project type. This is the largest field dataset for planning fallacy.
- **Software**: Jørgensen & Shepperd (2007, *IEEE Transactions on Software Engineering*): 90th percentile software project completion time was 1.9× the median estimate.

---

## 1.6 Category V: Social Influence Biases (Deliberation Context)

### 1.6.1 Groupthink

**Definition**: Suppression of critical evaluation in favor of group cohesion; illusion of unanimity; self-censorship (see also the Group Deliberation Survey for full treatment).

**Field-condition evidence**:
- **Intelligence failures**: Bay of Pigs (1961), Challenger launch decision (1986) — documented groupthink cycles (Janis, 1982; Vaughan, 1996).
- **Corporate governance**: Sims & Brinkmann (2003, *Journal of Business Ethics*): Enron board dynamics showed all 8 Janis groupthink symptoms retrospectively.

---

### 1.6.2 Social Proof / Herding

**Definition**: Adjusting beliefs or actions to match the perceived majority; informational cascades.

**Field-condition evidence**:
- **Financial crashes**: Bikhchandani & Sharma (2001, *IMF Staff Papers*): Herding in emerging market crises (Mexico 1994, Asia 1997) documented via portfolio correlation statistics; herding coefficient β ≈ 0.30–0.55 across asset classes.
- **Medical practice**: Phelps & Parente (1990, *Journal of Medical Decision Making*): Physician prescribing patterns show geographic clustering inconsistent with patient population differences — consistent with social learning/herding.

---

## 1.7 Full Reference Effect-Size Summary Table

| Bias | Domain | Best Field Study | Effect Size | Metric |
|---|---|---|---|---|
| Anchoring | Legal sentencing | Englich et al. (2006) | *d* ≈ 0.89 | Sentence length difference |
| Anchoring | Real estate | Northcraft & Neale (1987) | 12% per unit | Valuation shift |
| Overconfidence (calibration) | Expert forecasting | Tetlock (2005) | ~50% CI hit for 90% CI | Hit rate |
| Overconfidence (trading) | Financial markets | Barber & Odean (2001) | −3.7% annual | Return disadvantage |
| Planning fallacy | Infrastructure projects | Flyvbjerg et al. (2002) | 28–50% cost overrun | Project cost |
| Confirmation bias | Criminal justice | Charman (2013) | 2.5:1 ratio | Evidence interpretation |
| Hindsight bias | Legal decisions | Kamin & Rachlinski (1995) | *d* ≈ 0.67 | Negligence rating |
| Loss aversion (λ) | Real estate | Genesove & Mayer (2001) | λ ≈ 2–3 | Listing price premium |
| Loss aversion | Golf | Pope & Schweitzer (2011) | *d* ≈ 0.35 | Putt accuracy |
| Status quo / default | Organ donation | Johnson & Goldstein (2003) | 12% vs 99% | Consent rate |
| Status quo / default | Retirement savings | Madrian & Shea (2001) | +37 pp | Participation rate |
| Base rate neglect | Medical testing | Gigerenzer & Hoffrage (1995) | 72% error rate | Physician error |
| Framing | Medical decisions | McNeil et al. (1982) | 44–72% preference reversal | Choice reversal |
| Availability | Insurance purchases | Kunreuther et al. (1978) | Large spike post-disaster | Purchase rate |
| Sunk cost | Business | Staw (1981) | 40% excess investment | Resource allocation |

---

## 1.8 Key Citations

- Arkes, H. R., & Blumer, C. (1985). The psychology of sunk cost. *Organizational Behavior and Human Decision Processes*, 35(1), 124–140.
- Barber, B. M., & Odean, T. (2001). Boys will be boys: Gender, overconfidence, and common stock investment. *Quarterly Journal of Economics*, 116(1), 261–292.
- Camerer, C., Babcock, L., Loewenstein, G., & Thaler, R. (1997). Labor supply of New York City cab drivers. *Quarterly Journal of Economics*, 112(2), 407–441.
- Charman, S. D. (2013). The forensic confirmation bias. *Psychological Science*, 24(3), 356–360.
- Englich, B., Mussweiler, T., & Strack, F. (2006). Playing dice with criminal sentences. *Personality and Social Psychology Bulletin*, 32(2), 188–200.
- Fischhoff, B. (1975). Hindsight ≠ foresight. *Journal of Experimental Psychology: Human Perception and Performance*, 1(3), 288–299.
- Flyvbjerg, B., Holm, M. S., & Buhl, S. (2002). Underestimating costs in public works projects. *Journal of the American Planning Association*, 68(3), 279–295.
- Genesove, D., & Mayer, C. (2001). Loss aversion and seller behavior. *Quarterly Journal of Economics*, 116(4), 1233–1260.
- Gigerenzer, G., & Hoffrage, U. (1995). How to improve Bayesian reasoning without instruction. *Psychological Review*, 102(4), 684–704.
- Gilovich, T., Griffin, D., & Kahneman, D. (Eds.) (2002). *Heuristics and Biases: The Psychology of Intuitive Judgment.* Cambridge University Press.
- Johnson, E. J., & Goldstein, D. (2003). Do defaults save lives? *Science*, 302(5649), 1338–1339.
- Kahneman, D. (2011). *Thinking, Fast and Slow.* Farrar, Straus and Giroux.
- Kahneman, D., & Tversky, A. (1979). Prospect theory. *Econometrica*, 47(2), 263–291.
- Kamin, K. A., & Rachlinski, J. J. (1995). Ex post ≠ ex ante. *Law and Human Behavior*, 19(1), 89–104.
- Madrian, B. C., & Shea, D. F. (2001). The power of suggestion. *Quarterly Journal of Economics*, 116(4), 1149–1187.
- McNeil, B. J., Pauker, S. G., Sox, H. C., & Tversky, A. (1982). On the elicitation of preferences for alternative therapies. *New England Journal of Medicine*, 306(21), 1259–1262.
- Northcraft, G. B., & Neale, M. A. (1987). Experts, amateurs, and real estate. *Organizational Behavior and Human Decision Processes*, 39(1), 84–97.
- Pope, D. G., & Schweitzer, M. E. (2011). Is Tiger Woods loss averse? *American Economic Review*, 101(1), 129–157.
- Pronin, E., Lin, D. Y., & Ross, L. (2002). The bias blind spot. *Personality and Social Psychology Bulletin*, 28(3), 369–381.
- Staw, B. M. (1981). The escalation of commitment to a course of action. *Academy of Management Review*, 6(4), 577–587.
- Tetlock, P. E. (2005). *Expert Political Judgment.* Princeton University Press.
- Tversky, A., & Kahneman, D. (1974). Judgment under uncertainty: Heuristics and biases. *Science*, 185(4157), 1124–1131.
