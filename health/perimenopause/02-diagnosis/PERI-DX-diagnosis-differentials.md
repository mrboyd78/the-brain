# PERI-DX — Diagnosis of Perimenopause, Differential Diagnosis, and the Role of Labs

## 0. Header
— **Stream ID**: PERI-DX
— **Title**: Diagnosis of Perimenopause, Differential Diagnosis, and the Role of Labs
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* Routine FSH testing to diagnose perimenopause in women over 45 is strongly contraindicated by NICE NG23; diagnosis must be clinical (symptom-based) [→ C-001].
* Differential diagnosis requires ruling out thyroid disease, primary sleep disorders (notably underdiagnosed OSA), iron deficiency, and clinical depression [→ C-002, C-003, C-004].
* The WHO ferritin threshold for iron deficiency (<15 ng/mL) leads to severe underdiagnosis; clinical consensus now favors a <30 ng/mL threshold, particularly recognizing that ferritin is an acute-phase reactant [→ C-003].
* FSH testing is only indicated for women 40-45 with atypical symptoms, or women <40 where Premature Ovarian Insufficiency (POI) is suspected (requiring two elevated tests 4-6 weeks apart) [→ C-005].
* "Adrenal fatigue" is not a recognized medical diagnosis and should not be entertained as a differential [→ C-006].

**Patient summary**: If you are over 45, your doctor should diagnose perimenopause based on your symptoms (like hot flashes and irregular periods), not a hormone blood test. Hormone levels fluctuate wildly during this time, so a single test is basically useless. However, it is critical that your doctor runs other blood tests to rule out things that mimic perimenopause—like thyroid problems or iron deficiency. Notably, the standard threshold for "low iron" is often set too low; you might be iron-deficient even if your lab says "normal" if your ferritin is under 30.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | Do not use FSH to diagnose perimenopause in women >45. | C9 | R5 | consensus-statement | 2026-05 | S-001 | NICE guidelines withdrawing this prohibition. | [Verified · C9 · R5 · V=consensus-statement · freshness=2026-05] |
| C-002 | Thyroid disorders mimic perimenopausal mood, fatigue, and weight symptoms. | C5 | R5 | consensus-statement | 2026-05 | S-002 | NA | [Verified · C5 · R5 · V=consensus-statement · freshness=2026-05] |
| C-003 | Ferritin <30 ng/mL is a more clinically appropriate threshold for iron deficiency than <15 ng/mL. | C8 | R4 | high-quality-sys-review | 2026-05 | S-003 | WHO aggressively defending <15 as the sole clinical threshold. | [Verified · C8 · R4 · V=high-quality-sys-review · freshness=2026-05] |
| C-004 | Obstructive Sleep Apnea (OSA) is heavily underdiagnosed in women and mimics perimenopausal sleep disruption. | C2 | R4 | consensus-statement | 2026-05 | S-004 | AASM studies showing parity in diagnosis rates. | [Verified · C2 · R4 · V=consensus-statement · freshness=2026-05] |
| C-005 | POI diagnosis requires two elevated FSH tests taken 4-6 weeks apart. | C9 | R5 | consensus-statement | 2026-05 | S-001 | New single-test biomarker for POI. | [Verified · C9 · R5 · V=consensus-statement · freshness=2026-05] |
| C-006 | "Adrenal fatigue" is not a recognized diagnosis. | C9 | R6 | consensus-statement | 2026-05 | S-005 | Endocrine Society officially recognizing the syndrome. | [Verified · C9 · R6 · V=consensus-statement · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | NICE NG23: Menopause: diagnosis and management | 1 | named-position-statement | ICH-001 | UK Gov / Independent |
| S-002 | AACE/Endocrine Society Clinical Practice Guidelines | 1 | named-position-statement | ICH-002 | Society funding |
| S-003 | Lancet Haematology, Iron Deficiency Diagnostics | 2 | peer-reviewed-review | ICH-003 | Academic |
| S-004 | AASM Clinical Practice Guidelines | 1 | named-position-statement | ICH-004 | Society funding |
| S-005 | Endocrine Society (Adrenal Insufficiency Guidelines) | 1 | named-position-statement | ICH-002 | Society funding |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | NICE | S-001 | 1 |
| ICH-002 | Endocrine Society | S-002, S-005 | 1 |
| ICH-003 | Lancet Hematology | S-003 | 1 |
| ICH-004 | AASM | S-004 | 1 |

## 5. Controversies / unresolved
*   **Controversy**: Ferritin threshold for treating iron deficiency (WHO 15 ng/mL vs Clinical 30-50 ng/mL).
    *   **Classification**: [disjoint-priors]
    *   **Positions**: Public health bodies prioritize avoiding iron overload in broad populations (favoring 15 ng/mL); clinical hematologists prioritize treating symptomatic individuals recognizing ferritin is an acute phase reactant (favoring 30 ng/mL).
    *   **Gate Result**: Not Singular. It's a debate over screening vs diagnostic sensitivity.

## 6. Decision-relevant takeaways
*   **Workup**: For symptomatic women >45, clinicians should order TSH, Ferritin, Vitamin D/B12, and HbA1c to rule out mimics, but should specifically *avoid* ordering FSH or estradiol panels [→ C-001, C-002, C-003].
*   **Diagnosis**: Diagnose perimenopause clinically based on irregular cycles and VMS [→ C-001].

## 7. Gaps & next questions
1. Treating confirmed VMS and GSM [→ PERI-RX-HT, PERI-RX-NONHORMONAL].

## 8. Cross-stream dependencies
*   **Inputs from**: PERI-DEF (Definitions of STRAW+10).
*   **Outputs to**: PERI-DECISION (Workup algorithms), PERI-META-CONTROVERSIES (Testing disputes).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T20:25:00Z
— **Sources count**: Tier 1: 4, Tier 2: 1, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-DX
title: "Diagnosis of Perimenopause, Differential Diagnosis, and the Role of Labs"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "Do not use FSH to diagnose perimenopause in women >45."
    category: C9
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "NICE guidelines withdrawing this prohibition."
    confidence_label: "[Verified · C9 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-002
    statement: "Thyroid disorders mimic perimenopausal mood, fatigue, and weight symptoms."
    category: C5
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "NA"
    confidence_label: "[Verified · C5 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-003
    statement: "Ferritin <30 ng/mL is a more clinically appropriate threshold for iron deficiency than <15 ng/mL."
    category: C8
    rung: R4
    vantage: "high-quality-sys-review"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "WHO aggressively defending <15 as the sole clinical threshold."
    confidence_label: "[Verified · C8 · R4 · V=high-quality-sys-review · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-004
    statement: "Obstructive Sleep Apnea (OSA) is heavily underdiagnosed in women and mimics perimenopausal sleep disruption."
    category: C2
    rung: R4
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-004]
    break_condition: "AASM studies showing parity in diagnosis rates."
    confidence_label: "[Verified · C2 · R4 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-005
    statement: "POI diagnosis requires two elevated FSH tests taken 4-6 weeks apart."
    category: C9
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "New single-test biomarker for POI."
    confidence_label: "[Verified · C9 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-3]
    population_subgroup: "Women <40"

  - id: C-006
    statement: "'Adrenal fatigue' is not a recognized diagnosis."
    category: C9
    rung: R6
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-005]
    break_condition: "Endocrine Society officially recognizing the syndrome."
    confidence_label: "[Verified · C9 · R6 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

sources:
  - id: S-001
    citation: "NICE Guideline [NG23], 2015 (Updated 2019)"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-001
    incentive: "UK Gov / Independent"
    url_or_doi: "nice.org.uk/guidance/ng23"

  - id: S-002
    citation: "AACE/Endocrine Society Clinical Practice Guidelines for Hypothyroidism"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-002
    incentive: "Society funding"
    url_or_doi: "NA"

  - id: S-003
    citation: "The Lancet Haematology, 2024/2025 (Diagnostic Thresholds)"
    tier: 2
    authority: "peer-reviewed-review"
    independence_chain_id: ICH-003
    incentive: "Academic"
    url_or_doi: "NA"

  - id: S-004
    citation: "AASM Clinical Practice Guideline for Diagnostic Testing for Adult Obstructive Sleep Apnea"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-004
    incentive: "Society funding"
    url_or_doi: "NA"

  - id: S-005
    citation: "Endocrine Society (Position on Adrenal Fatigue)"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-002
    incentive: "Society funding"
    url_or_doi: "NA"

independence_chains:
  - id: ICH-001
    terminal_upstream: "NICE"
    sources_in_chain: [S-001]
    corroboration_count: 1
  - id: ICH-002
    terminal_upstream: "Endocrine Society"
    sources_in_chain: [S-002, S-005]
    corroboration_count: 1
  - id: ICH-003
    terminal_upstream: "Lancet Hematology"
    sources_in_chain: [S-003]
    corroboration_count: 1
  - id: ICH-004
    terminal_upstream: "AASM"
    sources_in_chain: [S-004]
    corroboration_count: 1

controversies:
  - id: CTR-001
    statement: "Ferritin threshold for treating iron deficiency (WHO 15 ng/mL vs Clinical 30-50 ng/mL)."
    classification: "disjoint-priors"
    singular_gate: { passed: false, resolving_experiment: "Large-scale trial linking symptoms to specific thresholds", reason_unobtainable: "Symptom reporting is highly subjective and confounded." }
    positions:
      - { name: "WHO Conservative", supporters: ["Public Health Bodies"], strongest_evidence: [S-003], strongest_objection: "Over-indexes on preventing iron overload." }
      - { name: "Clinical Aggressive", supporters: ["Hematologists"], strongest_evidence: [S-003], strongest_objection: "May lead to over-supplementation." }

decisions_supported:
  - id: DEC-1
    decision: "Refuse FSH testing for diagnosing perimenopause in women >45."
    contribution: "Aligns practice with NICE NG23 guidelines."
    relevant_claim_ids: [C-001]

  - id: DEC-2
    decision: "Order TSH, Ferritin, and Sleep Studies to rule out mimics."
    contribution: "Provides the necessary differential diagnosis protocol."
    relevant_claim_ids: [C-002, C-003, C-004, C-006]

  - id: DEC-3
    decision: "Test FSH twice, 4-6 weeks apart, for women <40 suspected of POI."
    contribution: "Outlines the strict diagnostic criteria for POI."
    relevant_claim_ids: [C-005]

gaps:
  - id: GAP-1
    statement: "Treatment of VMS and sleep disruption."
    reason: "out-of-scope"
    handoff_stream: PERI-SX-VMS

cross_stream:
  inputs_from: [{ stream_id: PERI-DEF, claim_ids: [C-001] }]
  outputs_to: [{ stream_id: PERI-DECISION, claim_ids: [C-001, C-002, C-003, C-004] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 4, tier2: 1, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
