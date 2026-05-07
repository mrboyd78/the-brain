# PERI-RX-NONHORMONAL — Non-Hormonal Prescriptions for Vasomotor Symptoms

## 0. Header
— **Stream ID**: PERI-RX-NONHORMONAL
— **Title**: Non-Hormonal Prescriptions for Vasomotor Symptoms
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* Fezolinetant (Veozah) is an FDA-approved NK3R antagonist that directly blocks the neurokinin B signaling pathway driving VMS, bypassing estrogen receptors entirely [→ C-001].
* Fezolinetant requires strict hepatic monitoring (baseline, months 1, 2, 3, 6, and 9) due to a labeled risk of hepatotoxicity (transaminase elevations observed in 1.5-2.3% of trial participants) [→ C-002].
* Low-dose paroxetine mesylate (7.5 mg daily) is the only SSRI formally FDA-approved for VMS, supported by Level I clinical evidence from NAMS [→ C-003].
* Gabapentin is supported by Level I evidence for off-label VMS treatment, while Oxybutynin carries Level I-II evidence but is flagged for potential cognitive decline risks with long-term use in older adults [→ C-004].

**Patient summary**: If you cannot or choose not to take hormone therapy for hot flashes, there are proven prescription alternatives. A newer class of drugs called NK3R antagonists (like Veozah) block the specific brain chemical that causes hot flashes, though they require regular blood tests to ensure your liver remains healthy. Alternatively, a very low-dose version of the antidepressant paroxetine is FDA-approved specifically for hot flashes, and doctors sometimes use nerve medications like gabapentin to quiet the nervous system at night.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | Fezolinetant reduces VMS by antagonizing NK3 receptors in the hypothalamus. | C5 | R5 | mechanism-plus-rct | 2026-05 | S-001 | SKYLIGHT trials refuted by post-market analysis. | [Verified · C5 · R5 · V=mechanism-plus-rct · freshness=2026-05] |
| C-002 | Fezolinetant carries hepatotoxicity risks requiring monitoring up to 9 months. | C2 | R6 | regulatory-agency | 2026-05 | S-001 | FDA removing the black box/warning label. | [Verified · C2 · R6 · V=regulatory-agency · freshness=2026-05] |
| C-003 | Low-dose paroxetine (7.5mg) has Level I evidence for VMS. | C9 | R5 | consensus-statement | 2026-05 | S-002 | NAMS downgrading the evidence. | [Verified · C9 · R5 · V=consensus-statement · freshness=2026-05] |
| C-004 | Long-term oxybutynin use for VMS is associated with cognitive decline risks. | C6 | R4 | consensus-statement | 2026-05 | S-002 | Long-term safety trials proving cognitive neutrality. | [Verified · C6 · R4 · V=consensus-statement · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | Fezolinetant (Veozah) FDA Label & SKYLIGHT Trials | 1 | regulatory-agency | ICH-001 | Industry/FDA |
| S-002 | 2023 Nonhormone Therapy Position Statement of NAMS | 1 | named-position-statement | ICH-002 | Society funding |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | FDA / Astellas Pharma | S-001 | 1 |
| ICH-002 | NAMS Consensus Panel | S-002 | 1 |

## 5. Controversies / unresolved
*   **Controversy**: Systemic adoption of NK3R antagonists over generic SSRIs.
    *   **Classification**: [disjoint-priors]
    *   **Positions**: Efficacy and mechanism strongly favor NK3R antagonists (treating the root neurochemical cause of VMS). However, cost, insurance gating, and the burden of serial liver blood draws make generic SSRIs/Gabapentin the pragmatic first-line non-hormonal choice for many practitioners.
    *   **Gate Result**: Not Singular. Resolves as drug prices drop and long-term post-market liver safety is established.

## 6. Decision-relevant takeaways
*   **Prescribing Protocol (NK3R)**: Clinicians prescribing fezolinetant must secure baseline LFTs and build clinical workflows to recall patients at months 1, 2, 3, 6, and 9 for serial testing. Do not initiate if baseline transaminases are $\ge 2\times$ ULN [→ C-002].
*   **Prescribing Protocol (Anticholinergics)**: Exercise extreme caution prescribing off-label oxybutynin for VMS in older postmenopausal women due to anticholinergic cognitive burdens [→ C-004].

## 7. Gaps & next questions
1. Efficacy of non-prescription behavioral interventions (CBT-I, hypnosis) [→ PERI-RX-BEHAVIORAL].

## 8. Cross-stream dependencies
*   **Inputs from**: PERI-SX-VMS (Mechanism).
*   **Outputs to**: PERI-DECISION (Treatment algorithms).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T21:05:00Z
— **Sources count**: Tier 1: 2, Tier 2: 0, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-RX-NONHORMONAL
title: "Non-Hormonal Prescriptions for Vasomotor Symptoms"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "Fezolinetant reduces VMS by antagonizing NK3 receptors in the hypothalamus."
    category: C5
    rung: R5
    vantage: "mechanism-plus-rct"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "SKYLIGHT trials refuted by post-market analysis."
    confidence_label: "[Verified · C5 · R5 · V=mechanism-plus-rct · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-002
    statement: "Fezolinetant carries hepatotoxicity risks requiring monitoring up to 9 months."
    category: C2
    rung: R6
    vantage: "regulatory-agency"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "FDA removing the black box/warning label."
    confidence_label: "[Verified · C2 · R6 · V=regulatory-agency · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-003
    statement: "Low-dose paroxetine (7.5mg) has Level I evidence for VMS."
    category: C9
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "NAMS downgrading the evidence."
    confidence_label: "[Verified · C9 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-004
    statement: "Long-term oxybutynin use for VMS is associated with cognitive decline risks."
    category: C6
    rung: R4
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "Long-term safety trials proving cognitive neutrality."
    confidence_label: "[Verified · C6 · R4 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: "Older adults"

sources:
  - id: S-001
    citation: "Fezolinetant (Veozah) FDA Labeling"
    tier: 1
    authority: "regulatory-agency"
    independence_chain_id: ICH-001
    incentive: "Industry/FDA"
    url_or_doi: "NA"

  - id: S-002
    citation: "The 2023 nonhormone therapy position statement of NAMS"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-002
    incentive: "Society funding"
    url_or_doi: "NA"

independence_chains:
  - id: ICH-001
    terminal_upstream: "FDA / Astellas Pharma"
    sources_in_chain: [S-001]
    corroboration_count: 1
  - id: ICH-002
    terminal_upstream: "NAMS Consensus Panel"
    sources_in_chain: [S-002]
    corroboration_count: 1

controversies:
  - id: CTR-001
    statement: "NK3R Antagonists vs generic SSRIs as first-line."
    classification: "disjoint-priors"
    singular_gate: { passed: false, resolving_experiment: "Health economic RCT comparing QALYs of both pathways.", reason_unobtainable: "Requires long-term generic cost stabilization." }
    positions:
      - { name: "Mechanistic purity", supporters: ["Specialists"], strongest_evidence: [S-001], strongest_objection: "High cost and lab burden." }

decisions_supported:
  - id: DEC-1
    decision: "Mandate serial LFTs (0, 1, 2, 3, 6, 9 months) for all patients on fezolinetant."
    contribution: "Strict compliance with FDA safety labeling."
    relevant_claim_ids: [C-002]

  - id: DEC-2
    decision: "Avoid oxybutynin in patients at risk for cognitive decline."
    contribution: "Follows NAMS warnings on anticholinergic load."
    relevant_claim_ids: [C-004]

gaps:
  - id: GAP-1
    statement: "Behavioral and lifestyle interventions."
    reason: "out-of-scope"
    handoff_stream: PERI-RX-BEHAVIORAL

cross_stream:
  inputs_from: [{ stream_id: PERI-SX-VMS, claim_ids: [C-001] }]
  outputs_to: [{ stream_id: PERI-DECISION, claim_ids: [C-001, C-002, C-003] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 2, tier2: 0, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
