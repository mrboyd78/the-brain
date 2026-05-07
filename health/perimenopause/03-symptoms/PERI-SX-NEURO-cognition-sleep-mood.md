# PERI-SX-NEURO — Neurological Symptoms: Cognition, Sleep Architecture, and Mood

## 0. Header
— **Stream ID**: PERI-SX-NEURO
— **Title**: Neurological Symptoms — Cognition, Sleep Architecture, and Mood
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* "Brain fog" is physiologically real. SWAN data confirms objective, transient declines in processing speed and verbal memory during the menopausal transition, independent of sleep disruption [→ C-001].
* Frequent, physiologically monitored vasomotor symptoms (VMS) during sleep are associated with a higher burden of white matter hyperintensities (WMHs), linking severe hot flashes to cerebrovascular micro-damage [→ C-002].
* SWAN sleep architecture studies using polysomnography (PSG) demonstrate that menopausal insomnia is complex; sleep disturbance is often tied to the *recall* of waking from a hot flash rather than just the physiological sweat response itself [→ C-003].
* Hormone therapy (HT) does not prevent Alzheimer's or long-term cognitive decline and is not indicated for this purpose in the general menopausal population [→ C-004].

**Patient summary**: "Brain fog" is a real, measurable medical event, not just you feeling overwhelmed. As estrogen levels change, your brain's processing speed and verbal memory temporarily dip. While frustrating, research shows this is usually a temporary phase. Furthermore, severe hot flashes, especially at night, might be leaving tiny markers of damage on the brain's white matter. While hormone therapy is excellent for treating hot flashes, it is not recommended as a "brain protector" or dementia preventative.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | Transitioning women experience measurable, transient declines in processing speed and verbal memory. | C2 | R4 | longitudinal-cohort | 2026-05 | S-001 | A well-powered cohort demonstrating cognitive stability across FMP. | [Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05] |
| C-002 | Sleep VMS burden correlates with increased white matter hyperintensities (WMHs) on MRI. | C5 | R3 | cross-sectional-imaging | 2026-05 | S-002 | A larger longitudinal imaging study refuting the MsBrain findings. | [Verified · C5 · R3 · V=cross-sectional-imaging · freshness=2026-05] |
| C-003 | HT is not indicated for the prevention of cognitive decline or dementia. | C9 | R5 | consensus-statement | 2026-05 | S-003 | A long-term RCT demonstrating primary preventative cognitive benefit. | [Verified · C9 · R5 · V=consensus-statement · freshness=2026-05] |
| C-004 | Persistent menopausal insomnia correlates strongly with VMS recall, not just objective VMS. | C2 | R4 | longitudinal-cohort | 2026-05 | S-004 | PSG studies showing 1:1 correspondence of EEG awakening to SSC spikes. | [Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | Maki et al., Menopause / SWAN Cognitive Data | 1 | peer-reviewed-primary | ICH-001 | NIH/SWAN funded |
| S-002 | Thurston et al., MsBrain Study (WMH findings) | 1 | peer-reviewed-primary | ICH-002 | NIH funded |
| S-003 | Menopause Society (NAMS) HT Position Statement | 1 | named-position-statement | ICH-003 | Society funding |
| S-004 | SWAN Sleep Study (Polysomnography) | 1 | peer-reviewed-primary | ICH-001 | NIH/SWAN funded |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | SWAN Cohort Data | S-001, S-004 | 1 |
| ICH-002 | MsBrain Study Cohort | S-002 | 1 |
| ICH-003 | Menopause Society Consensus | S-003 | 1 |

## 5. Controversies / unresolved
*   **Controversy**: The "Critical Window" Hypothesis for HT and Cognition.
    *   **Classification**: [disjoint-priors]
    *   **Positions**: Some researchers (often citing Maki's early work and observational data) suggest HT given *early* in the transition may protect cognition, while late initiation harms it. Major medical societies, however, state that current RCT evidence (like KEEPS/ELITE) does not robustly support prescribing HT *solely* for cognition at any age.
    *   **Gate Result**: Not Singular. Pending larger, longer-term RCTs specifically powered for cognitive outcomes in early initiators.

## 6. Decision-relevant takeaways
*   **Validation**: Clinicians must validate patient complaints of "brain fog" as a known physiological phenomenon of the transition, rather than dismissing them as stress or prescribing unnecessary cognitive workups [→ C-001].
*   **Prescribing Limits**: Do not prescribe systemic hormone therapy using dementia prevention as the primary clinical rationale [→ C-003].

## 7. Gaps & next questions
1. Interactions of poor sleep architecture with long-term cardiovascular risk [→ PERI-RISK-LONGTERM].

## 8. Cross-stream dependencies
*   **Inputs from**: PERI-SX-VMS (Thermoregulatory mechanisms).
*   **Outputs to**: PERI-RX-HT (Prescribing guidelines).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T20:45:00Z
— **Sources count**: Tier 1: 4, Tier 2: 0, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-SX-NEURO
title: "Neurological Symptoms: Cognition, Sleep Architecture, and Mood"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "Transitioning women experience measurable, transient declines in processing speed and verbal memory."
    category: C2
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "A well-powered cohort demonstrating cognitive stability across FMP."
    confidence_label: "[Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-002
    statement: "Sleep VMS burden correlates with increased white matter hyperintensities (WMHs) on MRI."
    category: C5
    rung: R3
    vantage: "cross-sectional-imaging"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "A larger longitudinal imaging study refuting the MsBrain findings."
    confidence_label: "[Verified · C5 · R3 · V=cross-sectional-imaging · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: null

  - id: C-003
    statement: "HT is not indicated for the prevention of cognitive decline or dementia."
    category: C9
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "A long-term RCT demonstrating primary preventative cognitive benefit."
    confidence_label: "[Verified · C9 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-004
    statement: "Persistent menopausal insomnia correlates strongly with VMS recall, not just objective VMS."
    category: C2
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-004]
    break_condition: "PSG studies showing 1:1 correspondence of EEG awakening to SSC spikes."
    confidence_label: "[Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: null

sources:
  - id: S-001
    citation: "Maki PM et al., SWAN Cognitive Findings"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-001
    incentive: "NIH/SWAN funded"
    url_or_doi: "NA"

  - id: S-002
    citation: "Thurston RC et al., MsBrain Study"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-002
    incentive: "NIH funded"
    url_or_doi: "NA"

  - id: S-003
    citation: "The 2022 Hormone Therapy Position Statement of The North American Menopause Society"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-003
    incentive: "Society funding"
    url_or_doi: "NA"

  - id: S-004
    citation: "SWAN Sleep Study Investigators"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-001
    incentive: "NIH/SWAN funded"
    url_or_doi: "NA"

independence_chains:
  - id: ICH-001
    terminal_upstream: "SWAN Cohort Data"
    sources_in_chain: [S-001, S-004]
    corroboration_count: 1
  - id: ICH-002
    terminal_upstream: "MsBrain Study Cohort"
    sources_in_chain: [S-002]
    corroboration_count: 1
  - id: ICH-003
    terminal_upstream: "Menopause Society Consensus"
    sources_in_chain: [S-003]
    corroboration_count: 1

controversies:
  - id: CTR-001
    statement: "The 'Critical Window' Hypothesis for HT and Cognition."
    classification: "disjoint-priors"
    singular_gate: { passed: false, resolving_experiment: "20-year RCT of early vs late HT initiators powered specifically for neurocognitive outcomes.", reason_unobtainable: "Logistically and financially implausible." }
    positions:
      - { name: "Early Window Benefit", supporters: ["Some observational researchers"], strongest_evidence: [S-001], strongest_objection: "Healthy user bias in observational cohorts." }
      - { name: "Strict Indication", supporters: ["Medical Societies"], strongest_evidence: [S-003], strongest_objection: "Overly conservative reading of KEEPS/ELITE data." }

decisions_supported:
  - id: DEC-1
    decision: "Clinically validate 'brain fog' as a temporary, physiological state."
    contribution: "Prevents misdiagnosis of early-onset dementia."
    relevant_claim_ids: [C-001]

  - id: DEC-2
    decision: "Refuse HT prescriptions if the sole indication is dementia prevention."
    contribution: "Aligns with society consensus."
    relevant_claim_ids: [C-003]

gaps:
  - id: GAP-1
    statement: "Differentiation of transition mood shifts from Major Depressive Disorder."
    reason: "out-of-scope"
    handoff_stream: PERI-DX

cross_stream:
  inputs_from: [{ stream_id: PERI-SX-VMS, claim_ids: [C-002] }]
  outputs_to: [{ stream_id: PERI-RX-HT, claim_ids: [C-003] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 4, tier2: 0, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
