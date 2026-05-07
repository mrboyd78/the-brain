# PERI-SX-VMS — Vasomotor Symptoms: Phenomenology, Mechanism, Natural History

## 0. Header
— **Stream ID**: PERI-SX-VMS
— **Title**: Vasomotor Symptoms — Phenomenology, Mechanism, Natural History
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* The mechanism of VMS is driven by estrogen depletion causing hypertrophy of KNDy neurons in the hypothalamus. This triggers excessive neurokinin B (NKB) signaling to thermoregulatory centers, falsely signaling heat and triggering massive heat dissipation (vasodilation) [→ C-001].
* Symptomatic women suffer from a severely narrowed "thermoneutral zone"; tiny elevations in core body temperature instantly trigger severe sweating, measurable objectively via sternal skin conductance [→ C-002].
* SWAN data identifies four distinct VMS trajectories: Early Onset, Late Onset, Persistently High, and Persistently Low. Women do not experience a uniform "average" transition [→ C-003].
* The "Early Onset" trajectory is not merely a nuisance; it is physiologically linked to subclinical cardiovascular disease markers, such as increased carotid intima-media thickness (CIMT) [→ C-004].
* Black women are significantly overrepresented in the "Late Onset" and "Persistently High" trajectories compared to white women [→ C-005].

**Patient summary**: Hot flashes and night sweats aren't just "in your head"—they are caused by a specific group of neurons (KNDy neurons) in your brain malfunctioning when estrogen drops. This malfunction shrinks your body's "comfort zone." A tiny rise in core body temperature that you wouldn't have noticed five years ago now triggers an extreme emergency cooling response (the hot flash). Everyone experiences this differently: some get them early, some late, some continuously, and some rarely. Getting them very early might actually be a signal to check your heart health, as they are linked to early blood vessel changes.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | Estrogen depletion causes KNDy hypertrophy and NKB hypersecretion, driving VMS. | C5 | R5 | mechanism-plus-rct | 2026-05 | S-001, S-004 | Reversal of NK3R antagonist efficacy data. | [Verified · C5 · R5 · V=mechanism-plus-rct · freshness=2026-05] |
| C-002 | VMS involves a narrowed thermoneutral zone where small Tc rises trigger exaggerated heat dissipation. | C5 | R4 | physiologic-measurement | 2026-05 | S-002 | Sternal skin conductance data refuted. | [Verified · C5 · R4 · V=physiologic-measurement · freshness=2026-05] |
| C-003 | VMS follows four distinct trajectories: Early, Late, High, and Low. | C2 | R4 | longitudinal-cohort | 2026-05 | S-003 | A larger cohort refuting the Thurston models. | [Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05] |
| C-004 | Early onset VMS trajectory is associated with higher carotid intima-media thickness (CIMT). | C5 | R4 | longitudinal-cohort | 2026-05 | S-003 | Cardiometabolic confounders proving to be the sole driver. | [Verified · C5 · R4 · V=longitudinal-cohort · freshness=2026-05] |
| C-005 | Black women are overrepresented in Late and High trajectories. | C2 | R4 | longitudinal-cohort | 2026-05 | S-003 | Disparities traced entirely to measurement instrument bias. | [Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | Rance et al., KNDy Neuron Mechanism | 1 | peer-reviewed-primary | ICH-001 | Academic |
| S-002 | Freedman RR., Am J Hum Biol, 2001 (Thermoregulatory Zone) | 1 | peer-reviewed-primary | ICH-002 | Academic |
| S-003 | Thurston RC et al., SWAN VMS Trajectories | 1 | peer-reviewed-primary | ICH-003 | NIH/SWAN funded |
| S-004 | NK3R Antagonist (Fezolinetant) FDA Label | 1 | regulatory-agency | ICH-004 | Industry (for the label) |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | Rance Laboratory KNDy Models | S-001 | 1 |
| ICH-002 | Freedman Laboratory | S-002 | 1 |
| ICH-003 | SWAN Cohort Data | S-003 | 1 |
| ICH-004 | NK3R Antagonist Phase 3 Trials | S-004 | 1 |

## 5. Controversies / unresolved
*   **Controversy**: Is VMS an independent cardiovascular risk factor or simply a proxy marker of underlying metabolic shifts?
    *   **Classification**: [disjoint-priors]
    *   **Positions**: Thurston's group posits specific trajectories (Early Onset) may act as independent cardiovascular indicators. Other cardiologists argue the correlation is driven by shared risk factors (obesity, lipid changes) that simply co-occur during the transition.
    *   **Gate Result**: Not Singular. Can be resolved by controlling for all confounding metabolic markers over a sufficiently long longitudinal timeline.

## 6. Decision-relevant takeaways
*   **Treatment Path**: The validation of the KNDy/NKB pathway directly supports the use of NK3R antagonists (fezolinetant, elinzanetant) for patients who cannot or will not take systemic hormone therapy [→ C-001].
*   **Screening**: Patients presenting with severe, early-onset VMS should be flagged for proactive cardiovascular risk assessment (lipid panel, blood pressure monitoring) due to the CIMT association [→ C-004].

## 7. Gaps & next questions
1. Clinical efficacy and safety profiles of NK3R antagonists vs HT [→ PERI-RX-NONHORMONAL].
2. Overlap of VMS-induced sleep disruption with primary insomnia [→ PERI-SX-NEURO].

## 8. Cross-stream dependencies
*   **Inputs from**: PERI-EPI (VMS duration data).
*   **Outputs to**: PERI-RX-NONHORMONAL (Mechanism validation for targeted therapies).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T20:25:00Z
— **Sources count**: Tier 1: 4, Tier 2: 0, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-SX-VMS
title: "Vasomotor Symptoms — Phenomenology, Mechanism, Natural History"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "Estrogen depletion causes KNDy hypertrophy and NKB hypersecretion, driving VMS."
    category: C5
    rung: R5
    vantage: "mechanism-plus-rct"
    freshness: "2026-05"
    source_ids: [S-001, S-004]
    break_condition: "Reversal of NK3R antagonist efficacy data."
    confidence_label: "[Verified · C5 · R5 · V=mechanism-plus-rct · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-002
    statement: "VMS involves a narrowed thermoneutral zone where small Tc rises trigger exaggerated heat dissipation."
    category: C5
    rung: R4
    vantage: "physiologic-measurement"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "Sternal skin conductance data refuted."
    confidence_label: "[Verified · C5 · R4 · V=physiologic-measurement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: null

  - id: C-003
    statement: "VMS follows four distinct trajectories: Early, Late, High, and Low."
    category: C2
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "A larger cohort refuting the Thurston models."
    confidence_label: "[Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-004
    statement: "Early onset VMS trajectory is associated with higher carotid intima-media thickness (CIMT)."
    category: C5
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "Cardiometabolic confounders proving to be the sole driver."
    confidence_label: "[Verified · C5 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-005
    statement: "Black women are overrepresented in Late and High trajectories."
    category: C2
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "Disparities traced entirely to measurement instrument bias."
    confidence_label: "[Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: "SWAN US Cohort"

sources:
  - id: S-001
    citation: "Rance NE et al., KNDy Neurons Mechanism"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-001
    incentive: "Academic"
    url_or_doi: "NA"

  - id: S-002
    citation: "Freedman RR. Am J Hum Biol. 2001; 13(4)"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-002
    incentive: "Academic"
    url_or_doi: "10.1002/ajhb.1077"

  - id: S-003
    citation: "Thurston RC et al., Menopause. Various SWAN Publications"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-003
    incentive: "NIH/SWAN funded"
    url_or_doi: "NA"

  - id: S-004
    citation: "FDA Labeling for Fezolinetant (Veozah)"
    tier: 1
    authority: "regulatory-agency"
    independence_chain_id: ICH-004
    incentive: "Industry"
    url_or_doi: "NA"

independence_chains:
  - id: ICH-001
    terminal_upstream: "Rance Laboratory KNDy Models"
    sources_in_chain: [S-001]
    corroboration_count: 1
  - id: ICH-002
    terminal_upstream: "Freedman Laboratory"
    sources_in_chain: [S-002]
    corroboration_count: 1
  - id: ICH-003
    terminal_upstream: "SWAN Cohort Data"
    sources_in_chain: [S-003]
    corroboration_count: 1
  - id: ICH-004
    terminal_upstream: "NK3R Antagonist Phase 3 Trials"
    sources_in_chain: [S-004]
    corroboration_count: 1

controversies:
  - id: CTR-001
    statement: "Is VMS an independent cardiovascular risk factor?"
    classification: "disjoint-priors"
    singular_gate: { passed: false, resolving_experiment: "Large longitudinal trial perfectly controlling for all metabolic changes during transition.", reason_unobtainable: "Cannot decouple VMS from the profound metabolic shifts of estrogen loss in human subjects." }
    positions:
      - { name: "Independent Risk Marker", supporters: ["Thurston et al"], strongest_evidence: [S-003], strongest_objection: "Metabolic confounders." }

decisions_supported:
  - id: DEC-1
    decision: "Utilize NK3R antagonists for patients contraindicated for HT."
    contribution: "Provides the physiological mechanism justifying the class."
    relevant_claim_ids: [C-001]

  - id: DEC-2
    decision: "Perform heightened CV screening on patients with Early Onset VMS."
    contribution: "Links early VMS to subclinical vascular changes."
    relevant_claim_ids: [C-003, C-004]

gaps:
  - id: GAP-1
    statement: "Efficacy of HT and NK3R antagonists."
    reason: "out-of-scope"
    handoff_stream: PERI-RX-HT

cross_stream:
  inputs_from: [{ stream_id: PERI-EPI, claim_ids: [C-002] }]
  outputs_to: [{ stream_id: PERI-RX-NONHORMONAL, claim_ids: [C-001] }, { stream_id: PERI-RISK-LONGTERM, claim_ids: [C-004] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 4, tier2: 0, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
