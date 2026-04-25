# Deliverable 4: Structural vs. Cognitive Debiasing — A Systematic Comparison

> **Survey**: Cognitive Biases and Debiasing Interventions  
> **Focus**: Process-change interventions vs. decision-maker mind-change interventions  
> **Date Compiled**: April 2026

---

## 4.1 The Fundamental Distinction

All debiasing strategies fall into one of two meta-categories:

**Cognitive/Internal Debiasing**: Attempts to change *how the decision-maker thinks* — their awareness of biases, their deliberative processes, or their metacognitive monitoring capacity.

**Structural/External Debiasing**: Attempts to change *the conditions under which decisions are made* — the information format, the decision architecture, the procedural requirements, or the institutional incentives.

This distinction is not merely taxonomic. It maps onto a fundamental difference in reliability and ecological validity: structural interventions do not depend on the decision-maker's psychological state, motivation, cognitive load, or awareness of their own biases. Cognitive interventions do.

**Thaler & Sunstein (2008)** (*Nudge*) made the case for structural interventions on this basis: "if in doubt, make the error-prone option the default." The logic: institutional design can be revised once; individual minds must be continuously managed.

---

## 4.2 Why Cognitive Debiasing Has Structural Disadvantages

### 4.2.1 The Bias at the Moment of Decision

Cognitive debiasing attempts to modify System 2 processing (deliberate, effortful, slow). But most biases originate in System 1 (automatic, fast, associative). The sequence is:

1. System 1 generates an initial judgment instantly
2. System 2 is supposed to evaluate and potentially override it
3. But System 2 is: (a) often not engaged, (b) cognitively depleted in high-stakes conditions, (c) tends to rationalize System 1 outputs rather than override them

Training teaches System 2 to recognize bias — but System 2 must first activate *and* correctly identify that this particular decision instance triggers the bias. This requires:
- Metacognitive awareness that the bias is relevant
- Cognitive resources to engage in correction
- Correct identification of the direction and magnitude of the bias
- Appropriate correction strategy deployment

**Wilson & Brekke (1994)** established these four conditions as necessary; all four co-occurring in natural field conditions is uncommon.

### 4.2.2 Transfer Failure (See Deliverable 5 for Full Treatment)

Cognitive debiasing shows reliably poor transfer across domains. Training on anchoring in a salary negotiation context does not reliably produce de-anchored reasoning in a medical risk assessment context. The learning is domain-specific rather than structural.

### 4.2.3 Ego Depletion and Decision Fatigue

**Baumeister, Bratslavsky, Muraven & Tice (1998)**: Self-regulatory effort (including cognitive monitoring) depletes a shared resource. Later decisions in a session show more biased reasoning than earlier ones. Judges grant parole less frequently as the day progresses (Danziger, Levav & Avnaim-Pesso, 2011, *PNAS*). Cognitive debiasing requires exactly the self-regulatory resource that is most depleted in high-stakes, high-volume decision environments.

---

## 4.3 Structural Debiasing: Taxonomy and Evidence

### 4.3.1 Type I: Decision Architecture Redesign (Defaults, Friction, Choice Architecture)

**Target**: Status quo bias, default effects, loss aversion driven inertia

**Mechanism**: Remove friction from the desired option; add friction to the undesired option; or set the desired option as the default (no action = desired outcome).

**Evidence quality**: Tier 1 — large-scale field experiments with real behavioral outcomes.

| Intervention | Effect Size | Source |
|---|---|---|
| Opt-out organ donation | +65 to +80 pp | Johnson & Goldstein (2003) |
| 401(k) auto-enrollment | +37 pp | Madrian & Shea (2001) |
| Default green energy tariff | +25–40 pp | Goldstein et al. (2008, *Journal of Consumer Research*) |
| Default to generic drugs | ~+15 pp in prescribing | Patel et al. (2016, *JAMA Internal Medicine*) |

**Key advantage**: No requirement for decision-maker awareness, motivation, or training. Works even on experts. Works even under time pressure and fatigue.

**Critical limitation**: Only applicable to choice situations where a default is meaningful. Novel, complex, strategic decisions cannot be defaulted. Defaults can also be set in the wrong direction (in ethics context: manipulation concern).

### 4.3.2 Type II: Information Format Redesign

**Target**: Base rate neglect, probability miscalculation, framing effects

**Mechanism**: Present information in the format naturally suited to accurate human reasoning (natural frequencies), removing the format mismatch that causes bias.

**Evidence quality**: Tier 1–2 — controlled studies with professional populations.

| Intervention | Effect Size | Source |
|---|---|---|
| Probability → frequency format (medical) | 72% → 24% error rate (*d* ≈ 1.20) | Gigerenzer & Hoffrage (1995) |
| Icon arrays for risk communication | Significant improvement in patient comprehension | Ancker et al. (2006, *Journal of Health Communication*) |
| Frequency-framed drug risk labels | ~40% better comprehension | Woloshin et al. (2008) |

**Key advantage**: The bias is eliminated at the information presentation stage — before it can influence reasoning. No training required.

### 4.3.3 Type III: Process Checklists (Surgical Safety)

**Target**: Omission errors, communication failures, procedural non-compliance

**Mechanism**: Externalize critical decision steps into a mandatory checklist protocol.

**Evidence quality**: Tier 1 — WHO Surgical Safety Checklist RCT.

**Haynes et al. (2009)** (*New England Journal of Medicine*, 360, 491–499): 8 hospitals in 8 countries; WHO Safe Surgery Saves Lives Study. The 19-item checklist reduced:
- Death rate: 1.5% → 0.8% (47% reduction)
- Major complications: 11.0% → 7.0% (36% reduction)
- *p* < 0.001 for both

**Limitation for cognitive debiasing**: The surgical checklist reduces *omission errors* (forgetting a step) and *communication failures* (not confirming patient identity/allergies). It does not address higher-order *reasoning errors* (confirmation bias in diagnosis, anchoring in treatment selection). The two classes of medical error require different interventions.

**de Vries et al. (2010)** (*New England Journal of Medicine*): Checklist compliance → complication reduction confirmed in larger replication (6 hospitals, n = 11,574 surgeries).

### 4.3.4 Type IV: Structured Decision Procedures (Red-Teaming; Adversarial Collaboration)

**Target**: Confirmation bias; groupthink; overconfidence in organizational assessments

**Mechanism**: Formalize dissent as an institutional role. Someone is assigned to find the flaws, not to reach consensus.

**Evidence quality**: Tier 2–3 — laboratory studies with professional populations; field case evidence.

| Intervention | Effect Size | Source |
|---|---|---|
| Devil's advocate | *d* = 0.56 on decision quality | Schwenk (1990) meta |
| Dialectical inquiry | *d* = 0.51 | Schwenk (1990) meta |
| Red team (GJP) | +5–8% Brier score improvement | Chang et al. (2016) |
| Adversarial collaboration (Kahneman-Mlodinow 2009) | Reduced anchoring in financial auditing *d* ≈ 0.35 | Tetlock & Mitchell (2009, *Perspectives on Psychological Science*) |

**Key advantage**: Does not require each decision-maker to overcome their own biases — requires only that the institutional process includes a challenger. This separates debiasing from the individual's psychological state.

**Limitation**: Organizations consistently find it difficult to sustain adversarial roles; defenders of the plan resist genuine challenge; red-teamers become captured by the main team's groupthink over time. The structural intervention requires cultural maintenance.

---

## 4.4 Cognitive Debiasing: Taxonomy and Evidence

### 4.4.1 Type I: Interactive Bias Training (with Feedback)

**Target**: Multiple biases (Morewedge et al. 2015 covered anchoring, confirmation bias, FAE, representativeness, projection, overconfidence)

**Evidence quality**: Tier 2–3 — MTurk RCT; 2-month follow-up

**Effect size**: 31–32% bias reduction (interactive); 18–19% (passive video)

**Advantage**: Generalizes to trained bias types; 2-month persistence; cheaper than structural redesign for complex decisions

**Limitation**: 
- MTurk population, not professional high-stakes context
- No 12-month follow-up
- Doesn't address all biases
- Transfer to unanticipated bias contexts remains unknown

### 4.4.2 Type II: Reference Class Forecasting Training

**Target**: Planning fallacy; optimism bias

**Mechanism**: Teaches practitioners to identify a reference class and use base-rate data

**Evidence quality**: Tier 1–2 — field implementation data

**Effect size**: 40–55% reduction in forecast error in infrastructure planning (Flyvbjerg, 2006)

**Advantage**: Addresses one of the most costly biases in organizational decision-making

**Note**: Straddles structural and cognitive — it teaches a method (cognitive) that must be institutionally adopted and mandated to achieve field-level effectiveness (structural).

### 4.4.3 Type III: Frequency Format Training

**Target**: Base rate neglect; probability miscalculation

**Evidence quality**: Tier 2 — controlled professional education studies

**Effect size**: *d* ≈ 0.90 at 5-week follow-up (Sedlmeier & Gigerenzer, 2001)

**Advantage**: Durable, transferable within the domain of probabilistic reasoning; generalizes across medical, legal, and financial contexts

### 4.4.4 Type IV: "Consider the Opposite" (Structured Counterfactual)

**Target**: Anchoring; confirmation bias; overconfidence

**Mechanism**: Requires generating specific reasons why the current judgment could be wrong. Structure is critical — instructions must be specific, not generic.

**Evidence quality**: Tier 2–3 — multiple laboratory RCTs; mixed field results

**Evidence**:
- **Lord, Lepper & Preston (1984)**: Instructions to "consider the opposite" (specific) reduced belief polarization from confirming evidence: *d* ≈ 0.60
- **Anderson (1982)**: Explicitly imagining a counter-instance reduced the perseverance of beliefs even after debriefing
- **Galinsky & Mussweiler (2001)** (*Journal of Personality and Social Psychology*, 81(4), 657–669): Counterpart perspective-taking in negotiation produced significantly less anchoring than standard conditions. *d* ≈ 0.55 on first offers.

**Critical condition**: Specificity of the counter-consideration matters enormously. "Think about the other side" (generic) fails. "List 2–3 specific pieces of evidence that would suggest your current estimate is too high" (specific) works.

---

## 4.5 Direct Comparison: What the Evidence Says

| Dimension | Structural/Process | Cognitive/Training |
|---|---|---|
| **Effect magnitude** | Very large (defaults: 30–80 pp) | Moderate (training: 20–55% reduction in specific bias) |
| **Reliability** | High — does not depend on user motivation | Low to moderate — depends on awareness, motivation, energy |
| **Durability** | Permanent while in force (no decay) | 2 months confirmed; 12+ months largely unknown |
| **Breadth of bias coverage** | Narrow — each structural change addresses specific decision type | Broader in principle — training can address multiple biases |
| **Transfer across domains** | N/A — structural changes are domain-specific by design | Poor — training effects are largely task-specific |
| **Cost** | High for architecture redesign; low for format changes | Low for initial training; requires reinforcement |
| **Resistance to fatigue** | Immune — works regardless of decision-maker state | Vulnerable — effects collapse under time pressure, ego depletion |
| **Scalability** | High once implemented | High for training delivery; low for sustained behavior change |
| **Evidence quality (best case)** | Tier 1 (Haynes, Madrian, Johnson) | Tier 2 (Morewedge; Gigerenzer) |
| **Professional development context** | Requires institutional/process redesign | Amenable to individual training programs |

---

## 4.6 Heuer (1999) and ACH as Structural Cognitive Hybrid

**Richard Heuer's** *Psychology of Intelligence Analysis* (1999) and the CIA Tradecraft Primer (2009) propose a suite of Structured Analytic Techniques (SATs) that attempt to operationalize cognitive corrections as structured procedures — making them hybrid interventions.

**Analysis of Competing Hypotheses (ACH)**:
- Step 1: Generate all plausible alternative hypotheses
- Step 2: List all evidence and arguments
- Step 3: Create a matrix of hypotheses × evidence
- Step 4: Assess diagnostic value of each evidence item for each hypothesis
- Step 5: Identify hypotheses with most diagnostic evidence against them (elimination logic, not confirmation)
- Step 6: Re-examine sensitive items; note what might cause rank ordering to change
- Step 7: Report conclusion; explicitly flag minority views

**ACH as structural debiasing**: By making the elimination logic explicit and structured, ACH attempts to build confirmation bias resistance into the procedure itself — not relying on individual analysts' motivation to seek disconfirming evidence.

**Actual evidence for ACH** (from Deliverable 2): Lehner et al. (2008) found no accuracy improvement vs. unstructured analysis. The value of ACH may be more in its *audit trail* and *minority view documentation* functions than in accuracy improvement.

**Heuer's honest assessment** (1999, p. 95): "ACH does not guarantee correct analysis. It does guarantee that analysts will consider all the evidence and all the significant alternatives. The final judgment still requires a thoughtful assessment of intangible factors."

**Critical implication**: SATs are best understood as documentation and deliberation frameworks that reduce the *probability* of bias manifestation, not as accuracy-guaranteeing algorithms. This is a structurally valuable function even without accuracy gains, because it creates an audit record that allows post-hoc identification of analytical failures.

---

## 4.7 The Most Defensible Strategy: Structural + Targeted Cognitive

Based on the evidence, the optimal debiasing strategy is not either/or — it is a specific combination:

1. **For high-volume, repeated decisions**: Use structural defaults and format redesign (information architecture change — does not require individual effort)
2. **For complex, novel decisions**: Use procedural protocols (premortem, red-teaming, ACH) that externalize the cognitive correction into the process
3. **For forecasting and probabilistic reasoning**: Add reference class forecasting training + frequency format instruction (cognitive but with evidence of durability)
4. **For individual skill building**: Use interactive training with personalized feedback (Morewedge format), not passive awareness lectures
5. **Avoid**: Generic awareness training, passive presentations about biases, unstructured "consider multiple perspectives" instructions, decision journaling without structured feedback

---

## 4.8 Key Citations

- Baumeister, R. F., Bratslavsky, E., Muraven, M., & Tice, D. M. (1998). Ego depletion. *Journal of Personality and Social Psychology*, 74(5), 1252–1265.
- Danziger, S., Levav, J., & Avnaim-Pesso, L. (2011). Extraneous factors in judicial decisions. *Proceedings of the National Academy of Sciences*, 108(17), 6889–6892.
- Galinsky, A. D., & Mussweiler, T. (2001). First offers as anchors. *Journal of Personality and Social Psychology*, 81(4), 657–669.
- Haynes, A. B., et al. (2009). A surgical safety checklist to reduce morbidity and mortality. *New England Journal of Medicine*, 360(5), 491–499.
- Heuer, R. J. (1999). *Psychology of Intelligence Analysis.* Center for the Study of Intelligence.
- Lord, C. G., Lepper, M. R., & Preston, E. (1984). Considering the opposite. *Journal of Personality and Social Psychology*, 47(6), 1231–1243.
- Schwenk, C. R. (1990). Effects of devil's advocacy and dialectical inquiry. *Organizational Behavior and Human Decision Processes*, 47(1), 161–176.
- Tetlock, P. E., & Mitchell, G. (2009). Calibrating empirical uncertainty. *Perspectives on Psychological Science*, 4(3), 239–255.
- Thaler, R. H., & Sunstein, C. R. (2008). *Nudge: Improving Decisions About Health, Wealth, and Happiness.* Yale University Press.
- Wilson, T. D., & Brekke, N. (1994). Mental contamination and mental correction. *Psychological Bulletin*, 116(1), 117–142.
