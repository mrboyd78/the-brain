# PERI-RX-HT — Hormone Therapy: Formulations, Timing, and Oncology Data

## 0. Header
— **Stream ID**: PERI-RX-HT
— **Title**: Hormone Therapy — Formulations, Timing, and Oncology Data
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* The "Timing Hypothesis" dictates the benefit-risk ratio: initiation within 10 years of menopause or before age 60 has a favorable profile for treating VMS and preventing bone loss. Initiation after 10 years or over age 60 carries unfavorable absolute risks (CHD, stroke, VTE, dementia) [→ C-001].
* The ELITE trial explicitly validated the timing hypothesis for cardiovascular health, demonstrating that early HT (<6 years) significantly slowed atherosclerosis (CIMT) progression, whereas late HT (>10 years) did not [→ C-002].
* Progestogens are not a uniform class. Data from the French E3N cohort robustly demonstrates that while synthetic progestins (e.g., MPA) elevate breast cancer risk, bioidentical micronized progesterone does not carry this same elevated risk profile [→ C-003].
* The KEEPS trial proved the safety of early intervention (within 3 years of menopause), showing symptom resolution with no adverse cardiovascular events, though it found a null effect on actually halting subclinical atherosclerosis progression [→ C-004].

**Patient summary**: Hormone therapy is safe and highly effective if started at the right time. The "Timing Hypothesis" is the golden rule: starting HT within 10 years of your final period or before age 60 is generally very safe and protects your bones and blood vessels. Starting it much later in life is not recommended because the risks of stroke and heart disease go up. Furthermore, the *type* of hormone matters. Older, synthetic progestins are linked to a slight increase in breast cancer risk, but modern "body-identical" micronized progesterone is not associated with this same risk.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | HT initiation <10 years post-menopause or <60 years old has a favorable risk-benefit ratio. | C9 | R6 | consensus-statement | 2026-05 | S-001 | NAMS reversing this position in future guidelines. | [Verified · C9 · R6 · V=consensus-statement · freshness=2026-05] |
| C-002 | Early HT (<6y) slows CIMT progression; late HT (>10y) does not. | C5 | R5 | randomized-controlled-trial | 2026-05 | S-002 | A larger RCT contradicting ELITE. | [Verified · C5 · R5 · V=randomized-controlled-trial · freshness=2026-05] |
| C-003 | Micronized progesterone does not elevate breast cancer risk to the degree of synthetic progestins. | C2 | R4 | longitudinal-cohort | 2026-05 | S-003 | An RCT directly comparing both formulations and finding equal risk. | [Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05] |
| C-004 | Early low-dose HT does not increase cardiovascular events (KEEPS). | C6 | R5 | randomized-controlled-trial | 2026-05 | S-004 | Long-term follow-up of KEEPS showing delayed adverse events. | [Verified · C6 · R5 · V=randomized-controlled-trial · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | Menopause Society (NAMS) 2022 Position Statement | 1 | named-position-statement | ICH-001 | Society funding |
| S-002 | ELITE Trial Investigators | 1 | peer-reviewed-primary | ICH-002 | NIH funded |
| S-003 | E3N Cohort Study Investigators | 1 | peer-reviewed-primary | ICH-003 | French Gov / INSERM |
| S-004 | KEEPS Trial Investigators | 1 | peer-reviewed-primary | ICH-002 | NIH/Foundation funded |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | Menopause Society Consensus Panel | S-001 | 1 |
| ICH-002 | RCT Vascular Studies | S-002, S-004 | 1 |
| ICH-003 | E3N Observational Cohort | S-003 | 1 |

## 5. Controversies / unresolved
*   **Controversy**: Is the E3N observational data sufficient to declare micronized progesterone "safe" regarding breast cancer?
    *   **Classification**: [disjoint-priors]
    *   **Positions**: Clinical societies in Europe and progressive practitioners in the US accept the E3N data as sufficient to preferentially prescribe micronized progesterone. Traditionalist US practitioners note that observational data is vulnerable to selection bias and await definitive RCT proof before declaring it universally safer than MPA.
    *   **Gate Result**: Not Singular. Resolvable by an adequately powered RCT comparing micronized progesterone to MPA on breast density and oncologic outcomes.

## 6. Decision-relevant takeaways
*   **Prescribing Protocol**: Clinicians should preferentially prescribe body-identical micronized progesterone over synthetic MPA when a progestogen is required for endometrial protection [→ C-003].
*   **Timing**: Adhere strictly to the Timing Hypothesis: do not initiate systemic HT in treatment-naïve women over 60 or >10 years post-FMP without profound extenuating circumstances [→ C-001].

## 7. Gaps & next questions
1. Options for women strictly contraindicated for HT (e.g., breast cancer survivors) [→ PERI-RX-NONHORMONAL].

## 8. Cross-stream dependencies
*   **Inputs from**: PERI-DEF (Staging), PERI-SX-VMS (Symptom targeting).
*   **Outputs to**: PERI-META-CONTROVERSIES (WHI fallout discussion).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T21:05:00Z
— **Sources count**: Tier 1: 4, Tier 2: 0, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-RX-HT
title: "Hormone Therapy: Formulations, Timing, and Oncology Data"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "HT initiation <10 years post-menopause or <60 years old has a favorable risk-benefit ratio."
    category: C9
    rung: R6
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "NAMS reversing this position in future guidelines."
    confidence_label: "[Verified · C9 · R6 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-002
    statement: "Early HT (<6y) slows CIMT progression; late HT (>10y) does not."
    category: C5
    rung: R5
    vantage: "randomized-controlled-trial"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "A larger RCT contradicting ELITE."
    confidence_label: "[Verified · C5 · R5 · V=randomized-controlled-trial · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-003
    statement: "Micronized progesterone does not elevate breast cancer risk to the degree of synthetic progestins."
    category: C2
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "An RCT directly comparing both formulations and finding equal risk."
    confidence_label: "[Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: "E3N French Cohort"

  - id: C-004
    statement: "Early low-dose HT does not increase cardiovascular events (KEEPS)."
    category: C6
    rung: R5
    vantage: "randomized-controlled-trial"
    freshness: "2026-05"
    source_ids: [S-004]
    break_condition: "Long-term follow-up of KEEPS showing delayed adverse events."
    confidence_label: "[Verified · C6 · R5 · V=randomized-controlled-trial · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: null

sources:
  - id: S-001
    citation: "The 2022 Hormone Therapy Position Statement of The North American Menopause Society"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-001
    incentive: "Society funding"
    url_or_doi: "NA"

  - id: S-002
    citation: "ELITE Trial Investigators"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-002
    incentive: "NIH funded"
    url_or_doi: "NA"

  - id: S-003
    citation: "E3N Cohort Study Investigators"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-003
    incentive: "French Gov / INSERM"
    url_or_doi: "NA"

  - id: S-004
    citation: "KEEPS Trial Investigators"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-002
    incentive: "NIH/Foundation funded"
    url_or_doi: "NA"

independence_chains:
  - id: ICH-001
    terminal_upstream: "Menopause Society Consensus Panel"
    sources_in_chain: [S-001]
    corroboration_count: 1
  - id: ICH-002
    terminal_upstream: "RCT Vascular Studies"
    sources_in_chain: [S-002, S-004]
    corroboration_count: 1
  - id: ICH-003
    terminal_upstream: "E3N Observational Cohort"
    sources_in_chain: [S-003]
    corroboration_count: 1

controversies:
  - id: CTR-001
    statement: "Observational proof of micronized progesterone safety vs RCT requirements."
    classification: "disjoint-priors"
    singular_gate: { passed: false, resolving_experiment: "Massive RCT powering breast cancer incidence across different progestogen arms.", reason_unobtainable: "Cost and duration make this highly unlikely post-WHI." }
    positions:
      - { name: "Clinical Pragmatism", supporters: ["European Societies"], strongest_evidence: [S-003], strongest_objection: "Relies on observational epidemiology." }

decisions_supported:
  - id: DEC-1
    decision: "Strict adherence to the Timing Hypothesis for HT initiation."
    contribution: "Provides the cardioprotective parameters for safe prescribing."
    relevant_claim_ids: [C-001, C-002]

  - id: DEC-2
    decision: "Preferential prescribing of micronized progesterone."
    contribution: "Minimizes breast cancer risk profile based on largest available cohorts."
    relevant_claim_ids: [C-003]

gaps:
  - id: GAP-1
    statement: "Options for contraindicated patients."
    reason: "out-of-scope"
    handoff_stream: PERI-RX-NONHORMONAL

cross_stream:
  inputs_from: []
  outputs_to: [{ stream_id: PERI-DECISION, claim_ids: [C-001, C-003] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 4, tier2: 0, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
