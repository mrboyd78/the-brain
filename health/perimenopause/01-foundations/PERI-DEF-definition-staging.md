# PERI-DEF — Definition, Staging, and Endocrine Physiology of Perimenopause

## 0. Header
— **Stream ID**: PERI-DEF
— **Title**: Definition, Staging, and Endocrine Physiology of Perimenopause
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* The canonical framework for staging reproductive aging is STRAW+10, anchored to the Final Menstrual Period (FMP). The menopausal transition comprises Early (Stage -2: variable cycle length >7 days) and Late (Stage -1: amenorrhea interval ≥60 days) stages [→ C-001].
* Hormone trajectories do not map neatly to chronological age; they are anchored to the FMP. FSH begins to rise ~6.10 years prior to FMP, accelerating ~2.05 years prior [→ C-002].
* Anti-Müllerian hormone (AMH) decline serves as a more direct endocrine marker of ovarian follicular depletion than FSH, beginning a significant decline ~5 years before FMP [→ C-003].
* High variability of FSH and estradiol during the transition makes single-point hormone measurements unreliable for diagnosing specific menopausal stages [→ C-004].
* Obesity significantly attenuates the FSH rise and delays the initial measurable increase [→ C-005].

**Patient summary**: Perimenopause is defined primarily by changes in your menstrual cycle, not by blood tests. The transition officially starts when your cycle length varies by 7 days or more, and enters the "late" phase when you skip a period for 60 days. While hormones like FSH and estrogen are fluctuating wildly, these blood tests are unreliable for telling you "where you are" in the transition. The biological driver of this process is the natural depletion of ovarian follicles, which causes a steady drop in a hormone called AMH about five years before your final period.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | STRAW+10 late transition (Stage -1) is characterized by ≥60 days amenorrhea. | C9 | R5 | consensus-statement | 2026-05 | S-001 | A new global staging workshop superseding STRAW+10. | [Verified · C9 · R5 · V=consensus-statement · freshness=2026-05] |
| C-002 | FSH begins to increase ~6.10 years before FMP, accelerating ~2.05 years prior. | C2 | R4 | longitudinal-cohort | 2026-05 | S-002 | A larger multi-ethnic longitudinal cohort demonstrating different timing. | [Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05] |
| C-003 | AMH decline begins ~5 years prior to the FMP, representing direct follicular depletion. | C5 | R5 | longitudinal-cohort | 2026-05 | S-003 | Methodological shift in AMH assays invalidating historical SWAN data. | [Verified · C5 · R5 · V=longitudinal-cohort · freshness=2026-05] |
| C-004 | Variability of FSH/E2 makes single-point testing unreliable for transition staging. | C6 | R4 | longitudinal-cohort | 2026-05 | S-002 | Discovery of a highly stabilized testing cadence or new biomarker. | [Verified · C6 · R4 · V=longitudinal-cohort · freshness=2026-05] |
| C-005 | Obesity attenuates FSH rise and delays initial increase to ~5.45 years before FMP. | C2 | R3 | longitudinal-cohort | 2026-05 | S-002 | Confounding mechanisms found in adiposity studies. | [Verified · C2 · R3 · V=longitudinal-cohort · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | Harlow et al., Menopause, 2012; 19(4):387-395 | 1 | named-position-statement | ICH-001 | NIH/NIA funded, multiple societies |
| S-002 | Randolph et al., JCEM, 2011; 96(3):746-754 | 1 | peer-reviewed-primary | ICH-002 | NIH/SWAN funded |
| S-003 | Sowers et al., JCEM, 2008; 93(9):3478-3483 | 1 | peer-reviewed-primary | ICH-002 | NIH/SWAN funded |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | STRAW+10 Workshop consensus | S-001 | 1 |
| ICH-002 | SWAN longitudinal cohort | S-002, S-003 | 1 |

## 5. Controversies / unresolved
*   **Controversy**: Use of AMH for predicting age of FMP in clinical practice.
    *   **Classification**: [disjoint-priors]
    *   **Positions**: Some clinicians utilize AMH to offer predictive timelines to patients, while consensus guidelines (STRAW+10) limit AMH to a supporting/secondary criteria role due to assay variability and wide confidence intervals for individual prediction.
    *   **Gate Result**: Not Singular. It is a debate over predictive utility thresholds.

## 6. Decision-relevant takeaways
*   **Diagnosis/Workup**: Clinicians should stage perimenopause using menstrual cycle history (STRAW+10 criteria) rather than single-point FSH/Estradiol tests, which are highly variable and unreliable [→ C-001, C-004].
*   **Testing**: Routine hormone testing for the sole purpose of confirming perimenopause is not evidence-based and should not be standard practice [→ C-004].

## 7. Gaps & next questions
1. Clinical diagnostic application and differential diagnosis of these criteria [→ PERI-DX].
2. Symptomatic manifestations resulting from these endocrine changes [→ PERI-SX-VMS, etc.].

## 8. Cross-stream dependencies
*   **Inputs from**: None (Foundational stream).
*   **Outputs to**: PERI-DX (diagnostic criteria), PERI-EPI (demographic applications of STRAW+10).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T20:15:00Z
— **Sources count**: Tier 1: 3, Tier 2: 0, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-DEF
title: "Definition, Staging, and Endocrine Physiology of Perimenopause"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "STRAW+10 late transition (Stage -1) is characterized by ≥60 days amenorrhea."
    category: C9
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "A new global staging workshop superseding STRAW+10."
    confidence_label: "[Verified · C9 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-002
    statement: "FSH begins to increase ~6.10 years before FMP, accelerating ~2.05 years prior."
    category: C2
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "A larger multi-ethnic longitudinal cohort demonstrating different timing."
    confidence_label: "[Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-003
    statement: "AMH decline begins ~5 years prior to the FMP, representing direct follicular depletion."
    category: C5
    rung: R5
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "Methodological shift in AMH assays invalidating historical SWAN data."
    confidence_label: "[Verified · C5 · R5 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-004
    statement: "Variability of FSH/E2 makes single-point testing unreliable for transition staging."
    category: C6
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "Discovery of a highly stabilized testing cadence or new biomarker."
    confidence_label: "[Verified · C6 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-005
    statement: "Obesity attenuates FSH rise and delays initial increase to ~5.45 years before FMP."
    category: C2
    rung: R3
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "Confounding mechanisms found in adiposity studies."
    confidence_label: "[Verified · C2 · R3 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: null

sources:
  - id: S-001
    citation: "Harlow et al., Menopause, 2012; 19(4):387-395"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-001
    incentive: "NIH/NIA funded"
    url_or_doi: "10.1097/gme.0b013e31824d8f40"

  - id: S-002
    citation: "Randolph et al., JCEM, 2011; 96(3):746-754"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-002
    incentive: "NIH/SWAN funded"
    url_or_doi: "10.1210/jc.2010-1746"

  - id: S-003
    citation: "Sowers et al., JCEM, 2008; 93(9):3478-3483"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-002
    incentive: "NIH/SWAN funded"
    url_or_doi: "10.1210/jc.2008-0358"

independence_chains:
  - id: ICH-001
    terminal_upstream: "STRAW+10 Workshop consensus"
    sources_in_chain: [S-001]
    corroboration_count: 1
  
  - id: ICH-002
    terminal_upstream: "SWAN longitudinal cohort"
    sources_in_chain: [S-002, S-003]
    corroboration_count: 1

controversies:
  - id: CTR-001
    statement: "Use of AMH for predicting age of FMP in clinical practice."
    classification: "disjoint-priors"
    singular_gate: { passed: false, resolving_experiment: "Large-scale highly-standardized AMH assay trial predicting FMP within <12 month precision", reason_unobtainable: "Assay heterogeneity and biological variance currently limit precision." }
    positions:
      - { name: "Clinical prediction utility", supporters: ["Some predictive medicine advocates"], strongest_evidence: [S-003], strongest_objection: "Individual variance is too high for clinical staging use." }
      - { name: "Strict STRAW+10 framework", supporters: ["Consensus bodies"], strongest_evidence: [S-001], strongest_objection: "Ignores advances in AMH assay precision." }

decisions_supported:
  - id: DEC-1
    decision: "Stage perimenopause using STRAW+10 menstrual criteria instead of FSH/E2 tests."
    contribution: "Provides the evidence base invalidating single-point hormone testing."
    relevant_claim_ids: [C-001, C-004]

  - id: DEC-2
    decision: "Interpret declining AMH and rising FSH as mid-transition indicators."
    contribution: "Outlines the timeline of endocrine changes anchored to FMP."
    relevant_claim_ids: [C-002, C-003]

gaps:
  - id: GAP-1
    statement: "Diagnostic criteria and differential diagnosis rules."
    reason: "out-of-scope"
    handoff_stream: PERI-DX

cross_stream:
  inputs_from: []
  outputs_to: [{ stream_id: PERI-DX, claim_ids: [C-001, C-004] }, { stream_id: PERI-EPI, claim_ids: [C-001] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 3, tier2: 0, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
