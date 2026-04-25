# Deliverable 2: Critical Negative Findings — What Does Not Work

> **Survey**: Cognitive Biases and Debiasing Interventions  
> **Focus**: Failed replications, null effects, and empirically unsupported practices  
> **Date Compiled**: April 2026

---

## 2.1 Introduction: The Debiasing Efficacy Problem

The debiasing literature has a fundamental structural problem: interventions that appear effective in controlled laboratory conditions frequently fail when deployed in field settings. Lilienfeld, Ammirati & Landfield (2009) conducted the most systematic critical review of this problem, concluding that "scientific psychology knows relatively little about how to reliably minimize judgment error in applied contexts" — a damning assessment given decades of research.

This deliverable documents specific interventions that:
1. Have failed to replicate their original findings
2. Show null effects in field conditions despite laboratory success
3. Are widely taught or deployed in professional-development contexts without adequate empirical support
4. Show backfire effects — interventions that make bias *worse* under some conditions

---

## 2.2 The "Do Not Use" List: Failed and Unsupported Interventions

### 2.2.1 ❌ Generic Awareness Training ("Bias Education")

**Claimed mechanism**: Teaching people about cognitive biases makes them less susceptible to them.

**Actual evidence**: This is arguably the most robustly failed intervention in the debiasing literature.

**Specific failure evidence**:

**Fischhoff (1982)** (*Judgment Under Uncertainty: Heuristics and Biases*, Cambridge): The earliest systematic review found that simply informing subjects about overconfidence bias did not reduce it. Subjects told "people are typically overconfident in this task" still showed near-identical calibration errors.

**Weinstein (1980)** (*Journal of Personality and Social Psychology*, 39, 806–820): Warning subjects about optimism bias ("most people overestimate their chances of experiencing positive events") produced no measurable reduction in unrealistic optimism. *n* = 258 undergraduates; effect size ~0.

**Larrick, Morgan & Nisbett (1990)** (*Organizational Behavior and Human Decision Processes*, 45(3), 331–347): Teaching the statistical rule "regression to the mean" produced significant improvement only on problems with transparent structural similarity to trained examples; transfer to novel domains was negligible.

**Wilson & Brekke (1994)** (*Psychological Bulletin*, 116(1), 117–142) — "Mental contamination and mental correction": Reviewed 50+ debiasing studies. Conditions required for successful correction: (a) awareness of the unwanted judgment process, (b) motivation to correct, (c) ability to identify direction and magnitude of bias, (d) ability to adjust appropriately. All four conditions rarely co-occur in real-world settings.

**West et al. (2012)** (*Journal of Personality and Social Psychology*, 103(4), 550–569): Critical finding — **cognitive sophistication did not reduce susceptibility to cognitive biases**. People with higher SAT scores, higher cognitive reflection test scores, and greater actively open-minded thinking styles showed *equal* susceptibility to anchoring, framing, sunk costs, and 13 other biases. Higher sophistication sometimes increased the bias blind spot.

**Professional context**: Most corporate decision-quality training programs consist of awareness presentations about cognitive biases (a slide deck with examples). The existing evidence provides essentially no support for the effectiveness of this format for reducing actual decision error in practice.

**⚠️ Critical caveat**: The failure is specifically for **passive awareness** training. Interactive training with feedback (Morewedge et al., 2015) shows different results — see Deliverable 3.

---

### 2.2.2 ❌ Warnings Without Correction Mechanisms

**Claimed mechanism**: Alerting decision-makers that a specific bias may be active will cause them to correct for it.

**Actual evidence**:

**Chapman & Johnson (2002)** (*Heuristics and Biases*, Gilovich et al.): Warnings about anchoring ("don't be influenced by irrelevant numbers") produced no significant reduction in anchoring effects. Subjects given warnings showed slightly smaller anchoring effects than controls (Δ ≈ 0.08 on a 1–10 scale), but the difference was not statistically significant in most conditions.

**Condition-specificity**: Warnings are effective *only when* (a) they occur before the biasing information (not after), (b) they identify the specific source of bias, and (c) the judge is motivated and has cognitive resources to correct. Pre-anchoring warnings with high-effort instructions produce partial effects (*d* ≈ 0.25, Mussweiler, Strack & Pfeiffer, 2000).

**Physician context**: Mamede et al. (2010, *Academic Medicine*): Physicians warned specifically to "think twice" about their diagnoses showed no improvement in diagnostic accuracy on difficult clinical cases. The warning produced slower decisions but not more accurate ones.

---

### 2.2.3 ❌ Incentivizing Accuracy (in Many Contexts)

**Claimed mechanism**: Financial or reputational incentives motivate more careful reasoning, reducing bias.

**Actual evidence**: This is a *context-dependent* failure — incentives work for some biases and not others.

**What doesn't work**:
- **Anchoring under incentives**: Tversky & Kahneman (1974) original finding involved trivial stakes; Fudenberg, Levine & Maniadis (2012) found anchoring persisted for incoherent arbitrary anchors even with real monetary stakes in 3 out of 4 conditions.
- **Overconfidence under incentives**: Camerer & Hogarth (1999, *Journal of Risk and Uncertainty*, 19, 7–42) meta-analyzed 74 studies with financial incentives. Incentives **failed** to improve calibration in most tasks; they reduced overconfidence in some clerical production tasks (where more effort = better output) but not in judgment-under-uncertainty tasks (where more effort ≠ better calibration).
- **Confirmation bias under incentives**: Nickerson (1998, *Review of General Psychology*, 2(2), 175–220): Incentivizing accuracy did not reduce confirmation bias in hypothesis testing tasks; the bias persisted because motivated reasoners found confirming evidence more satisfying even under incentive schemes.

**Key distinction**: Incentives improve performance on *effort tasks* but not on *calibration tasks*. You cannot outperform your mental model by trying harder if the bias is structural, not motivational.

---

### 2.2.4 ❌ Merely Considering Multiple Perspectives (Without Structure)

**Claimed mechanism**: Asking decision-makers to "consider different viewpoints" or "think about multiple perspectives" reduces confirmation bias and one-sided thinking.

**Actual evidence**:

**Mussweiler & Bodenhausen (2002)** (*Personality and Social Psychology Bulletin*, 28(10), 1398–1407): Unstructured instructions to "consider different perspectives" on a target person actually increased stereotyping under some conditions — subjects drew on the most accessible (stereotypical) alternative perspective when asked to "think differently."

**Lord, Lepper & Preston (1984)** (*Journal of Personality and Social Psychology*, 47(6), 1231–1243): Subjects instructed to be "fair and unbiased" in evaluating conflicting studies on capital punishment showed *equal* belief polarization as control subjects — 76% vs. 74% adherence to prior position.

**Koriat, Lichtenstein & Fischhoff (1980)** (*Journal of Experimental Psychology: Human Perception and Performance*, 6(1), 107–118): Subjects who listed reasons for AND against their answers before confidence rating showed no significant improvement in calibration compared to those who only listed reasons for their answers. *d* ≈ 0.08 (ns).

**Critical nuance**: The failure is for *unstructured* perspective-taking. Structured forced-choice counterfactual techniques (e.g., requiring specific hypothetical alternatives) show different effects — see Deliverable 3.

---

### 2.2.5 ❌ Analysis of Competing Hypotheses (ACH) — Weaker Evidence Than Assumed

**Context**: ACH (Heuer, 1999) is the most widely taught structured analytic technique in the U.S. intelligence community and is mandated by the CIA Tradecraft Primer (2009). It involves listing hypotheses, listing evidence, and rating diagnostic value of each evidence item for each hypothesis.

**⚠️ The empirical evidence for ACH is substantially weaker than its use would suggest.**

**Critical evaluation**:

**Lehner, Adelman, Cheikes & Brown (2008)** (*Proceedings of the 5th International Conference on Intelligence Analysis*): A direct experimental comparison of ACH vs. unstructured analysis. ACH produced **no significant improvement in analytic accuracy** compared to control; analysts using ACH were not more correct than those using intuitive methods. The primary benefit was documentation of reasoning, not accuracy.

**Chang, Berdini, Tetlock & Chen (2018)** (*Proceedings of the AAAI Workshop on Expanding the Boundaries of Health Informatics*): Trained forecasters using ACH-style structured analysis did not outperform simple statistical baselines or intuitive judgment by trained forecasters.

**Structured analytic techniques (SATs) in general**: Dhami, Mandel, Mellers & Tetlock (2015, *Frontiers in Psychology*, 6, 61): Reviewed 12 SATs including ACH. Conclusion: "There is very little empirical evidence on whether SATs improve intelligence analysis." The evidence base was "weak, fragmented, and inconclusive" — the recommendation for practice outran the evidence by a significant margin.

**Key implication**: ACH is taught as if empirically validated but is essentially a reasonable heuristic procedure without robust field-condition evidence of accuracy improvement. The CIA's adoption of ACH represents an institutional commitment that predates — and has not been substantially updated with — controlled empirical evaluation.

---

### 2.2.6 ❌ Checklists for Diagnostic Accuracy in Medicine — Context-Specific Failure

**Context**: Checklists for surgical safety (WHO Surgical Safety Checklist) have strong efficacy evidence. However, checklists for *diagnostic* accuracy (intended to debias clinical reasoning) have weaker evidence than widely believed.

**Failure evidence**:

**Ely et al. (2011)** (*BMJ Quality and Safety*, 20(10), 836–845): A structured cognitive forcing strategy checklist distributed to emergency physicians produced no significant improvement in diagnostic error rates in a prospective observational study. Error rate: 12.5% (checklist) vs. 13.1% (control); *p* = ns.

**Mamede et al. (2012)** (*Academic Medicine*, 87(12), 1713–1719): Structured reflection prompted by checklist did not improve diagnosis of difficult clinical cases in experienced physicians. For straightforward cases, checklists produced slower but equally accurate diagnoses.

**Important distinction**: Checklists for *process compliance* (surgical safety, aviation pre-flight) work well. Checklists for *improving the quality of diagnostic reasoning* within System 1 processing show much weaker evidence.

---

### 2.2.7 ❌ Self-Insight / Reflective Journaling for Bias Reduction

**Claimed mechanism**: Reflective practice — writing about one's biases, keeping a decision journal — produces lasting reductions in biased thinking.

**Actual evidence**: The evidence base is essentially absent for field conditions.

**What exists**:
- Anecdotal and case study literature (prominently in popular management texts, e.g., Kahneman, 2011's suggestion of decision journals)
- No published RCT with pre/post measures comparing decision journal conditions to controls in professional settings
- **Kruger & Dunning (1999)** (*Journal of Personality and Social Psychology*, 77(6), 1121–1134): Metacognitive incompetence — those least skilled at a task are also least able to recognize their poor performance, undermining reflective practice in those who need it most

**Institutional context**: Many professional-development programs in investment banking, consulting, and strategy include "decision journal" recommendations. These are unvalidated by field-condition RCTs and the underlying theory (that self-reflection produces calibration) is undercut by the Dunning-Kruger and bias blind spot evidence.

---

### 2.2.8 ❌ Cognitive Debiasing Training Without Active Feedback

**Clarification on the Morewedge et al. (2015) finding**: The effective Morewedge intervention used *interactive games with personalized feedback*, not passive video or lecture. The study's own data show that:
- Passive instructional video: 18–19% bias reduction (maintained at 2 months)
- Active game with feedback: 32% bias reduction (maintained at 2 months)

**What fails**: Lecture/video without active practice and feedback. Many corporate training programs implement the "video" format but without the feedback mechanism that produces the larger effect.

**Critical replication gap**: The Morewedge (2015) study was conducted on MTurk workers evaluating hypothetical decisions. The transfer to high-stakes professional judgment in complex real-world environments has not been established in controlled studies.

---

## 2.3 Widely Taught in Professional Contexts But Lacking Support

| Intervention | Typical Context | Evidence Status | Best Counter-Evidence |
|---|---|---|---|
| "Be aware of your biases" training | Corporate L&D, MBA programs | ❌ No effect on bias | West et al. (2012); Fischhoff (1982) |
| ACH as accuracy tool | Intelligence analysis, military | ⚠️ No accuracy improvement shown | Lehner et al. (2008); Dhami et al. (2015) |
| "Consider multiple perspectives" | Consulting, strategy | ❌ No effect without structure | Lord et al. (1984); Mussweiler & Bodenhausen (2002) |
| Decision/reflection journals | Finance, consulting, coaching | ❌ No controlled evidence | Kruger & Dunning (1999) |
| Incentives for unbiased judgment | Performance management | ⚠️ Fails for calibration tasks | Camerer & Hogarth (1999) |
| Generic warnings ("don't anchor") | Sales training, negotiation | ❌ Minimal effect without specifics | Chapman & Johnson (2002) |
| "Slow down and think" instructions | Clinical decision support | ⚠️ Slower but not more accurate | Mamede et al. (2010, 2012) |
| Myers-Briggs as self-awareness tool | HR/L&D (widespread) | ❌ No validity evidence; low retest reliability | Pittenger (2005, *Journal of Career Assessment*) |
| Unconscious bias training (passive) | Corporate DEI programs | ⚠️ No behavioral change evidence | Forscher et al. (2019, *Journal of Personality and Social Psychology*) |

---

## 2.4 The Backfire Effect — When Debiasing Makes Things Worse

### 2.4.1 Overcorrection via "Consider the Opposite"

When subjects are asked to generate too many counterarguments, they experience difficulty and misattribute this difficulty as evidence that the original position is correct (negative ease-of-retrieval effect: Schwarz et al., 1991). Asking for 12 reasons why you might be wrong can paradoxically increase confidence in the original position.

**Evidence**: Schwarz et al. (1991, *Journal of Personality and Social Psychology*, 61(2), 195–202): Subjects who listed 12 examples of assertive behavior rated themselves as LESS assertive than those who listed 2 examples — effort led to self-doubt rather than recall success.

**Implication for facilitators**: "Consider the opposite" exercises should cap the number of generated counterarguments at 2–5. Requiring exhaustive challenge lists may backfire.

### 2.4.2 Demand Characteristics Masking Rather Than Reducing Bias

When people believe they are being evaluated for bias, they may *claim* to have updated their position (satisfying the demand characteristic) without actually changing their underlying judgment process. This produces apparent debiasing that vanishes when the demand is removed.

**Evidence**: Tetlock (1983, *Journal of Personality and Social Psychology*, 45(1), 74–83): Accountability-to-unknown-audience condition produced more balanced thinking; accountability to an *already-committed* audience produced *more* one-sided thinking. The effect depends entirely on audience and accountability structure.

---

## 2.5 The Unconscious Bias Training Problem

This deserves specific attention because passive unconscious bias training (UBT) is currently the most widely deployed debiasing program in organizations worldwide.

**Forscher et al. (2019)** (*Journal of Personality and Social Psychology*, 117(6), 1287–1324): The most comprehensive meta-analysis of implicit bias interventions to date (492 studies, 87,418 participants). Findings:
- Interventions can reliably change implicit bias scores (IAT scores)
- However, changes in implicit bias scores have **near-zero correlation** with changes in discriminatory behavior
- The correlation between IAT scores and discriminatory behavior was *r* = .15 in the meta-analysis
- **No intervention reliably reduced discriminatory behavior** across contexts

**Paluck, Porat, Clark & Green (2021)** (*Psychological Science*, 32(7), 1042–1055): Reviewed prejudice reduction interventions; most had weak or no evidence of behavioral impact.

**Institutional implication**: Organizations spending substantial resources on UBT are deploying an intervention with weak evidence for behavioral change while potentially creating a false sense of having addressed the problem.

---

## 2.6 Key Citations

- Camerer, C. F., & Hogarth, R. M. (1999). The effects of financial incentives in experiments. *Journal of Risk and Uncertainty*, 19(1–3), 7–42.
- Chapman, G. B., & Johnson, E. J. (2002). Incorporating the irrelevant. In T. Gilovich et al. (Eds.), *Heuristics and Biases.* Cambridge University Press.
- Dhami, M. K., Mandel, D. R., Mellers, B. A., & Tetlock, P. E. (2015). Improving intelligence analysis with decision science. *Perspectives on Psychological Science*, 10(6), 753–757.
- Fischhoff, B. (1982). Debiasing. In D. Kahneman, P. Slovic, & A. Tversky (Eds.), *Judgment Under Uncertainty.* Cambridge University Press.
- Forscher, P. S., Lai, C. K., Axt, J. R., Ebersole, C. R., Herman, M., Devine, P. G., & Nosek, B. A. (2019). A meta-analysis of procedures to change implicit measures. *Journal of Personality and Social Psychology*, 117(6), 1287–1324.
- Koriat, A., Lichtenstein, S., & Fischhoff, B. (1980). Reasons for confidence. *Journal of Experimental Psychology: Human Perception and Performance*, 6(1), 107–118.
- Kruger, J., & Dunning, D. (1999). Unskilled and unaware of it. *Journal of Personality and Social Psychology*, 77(6), 1121–1134.
- Lehner, P. E., Adelman, L., Cheikes, B. A., & Brown, M. J. (2008). Confirmation bias in complex analyses. *IEEE Transactions on Systems, Man, and Cybernetics*, 38(4), 584–592.
- Lilienfeld, S. O., Ammirati, R., & Landfield, K. (2009). Giving debiasing away. *Perspectives on Psychological Science*, 4(4), 390–398.
- Lord, C. G., Lepper, M. R., & Preston, E. (1984). Considering the opposite. *Journal of Personality and Social Psychology*, 47(6), 1231–1243.
- Mamede, S., Schmidt, H. G., Rikers, R. M. P. J., Penaforte, J. C., & Schmidt-Filho, J. (2010). Influence of perceived difficulty of cases on physicians' diagnostic reasoning. *Academic Medicine*, 85(6), 1003–1008.
- Schwarz, N., Bless, H., Strack, F., Klumpp, G., Rittenauer-Schatka, H., & Simons, A. (1991). Ease of retrieval as information. *Journal of Personality and Social Psychology*, 61(2), 195–202.
- West, R. F., Meserve, R. J., & Stanovich, K. E. (2012). Cognitive sophistication does not attenuate the bias blind spot. *Journal of Personality and Social Psychology*, 103(4), 550–569.
- Wilson, T. D., & Brekke, N. (1994). Mental contamination and mental correction. *Psychological Bulletin*, 116(1), 117–142.
