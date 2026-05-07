# PERI-RX-BEHAVIORAL — Behavioral Interventions for Vasomotor Symptoms

## 0. Header
— **Stream ID**: PERI-RX-BEHAVIORAL
— **Title**: Behavioral Interventions for Vasomotor Symptoms
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* Cognitive Behavioral Therapy (CBT) carries Level I evidence (highest recommendation) from NAMS for the management of VMS. CBT reduces the *distress* and perceived problem rating of hot flashes, though it may not significantly reduce the raw physiological frequency of the flashes themselves [→ C-001].
* Clinical Hypnosis is strongly recommended with Level I evidence from NAMS. Unlike CBT, trials demonstrate that hypnosis delivered by a trained practitioner significantly reduces both the *severity* and the *frequency* of hot flashes, while also improving sleep metrics [→ C-002].
* Cognitive Behavioral Therapy for Insomnia (CBT-I) is the recommended first-line behavioral intervention specifically for the management of menopause-related insomnia, distinguished from CBT targeting VMS [→ C-003].
* While widely utilized, paced respiration (deep breathing) lacks sufficient evidence from rigorous trials and is explicitly *not* recommended by NAMS as a standalone treatment for VMS [→ C-004].

**Patient summary**: If you want to manage hot flashes without medication, the two most proven therapies are Cognitive Behavioral Therapy (CBT) and Clinical Hypnosis. The Menopause Society gives both its highest rating. CBT works by changing how your brain processes the physical sensation, making hot flashes less distressing and less likely to ruin your day. Clinical Hypnosis, surprisingly, has been proven in trials to actually reduce how often you get hot flashes and how severe they are. Note that simple "deep breathing" exercises, while relaxing, are not scientifically proven to stop hot flashes.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | CBT is Level I recommended for reducing VMS distress. | C9 | R5 | consensus-statement | 2026-05 | S-001 | Future RCTs showing no superiority over control for distress. | [Verified · C9 · R5 · V=consensus-statement · freshness=2026-05] |
| C-002 | Clinical hypnosis reduces physiological VMS frequency (Level I evidence). | C5 | R5 | consensus-statement | 2026-05 | S-001 | Meta-analysis demonstrating hypnosis is purely placebo. | [Verified · C5 · R5 · V=consensus-statement · freshness=2026-05] |
| C-003 | CBT-I is a distinct, first-line intervention for menopause insomnia. | C9 | R5 | consensus-statement | 2026-05 | S-001 | Medical societies prioritizing pharmacology over CBT-I. | [Verified · C9 · R5 · V=consensus-statement · freshness=2026-05] |
| C-004 | Paced respiration is not recommended for VMS treatment due to lack of efficacy. | C9 | R4 | consensus-statement | 2026-05 | S-001 | New high-power RCT showing efficacy of respiration. | [Verified · C9 · R4 · V=consensus-statement · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | The 2023 Nonhormone Therapy Position Statement of NAMS | 1 | named-position-statement | ICH-001 | Society funding |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | NAMS Consensus Panel | S-001 | 0 |

## 5. Controversies / unresolved
*   **Controversy**: Hypnosis vs Placebo Effect in VMS.
    *   **Classification**: [disjoint-priors]
    *   **Positions**: Medical traditionalists are often skeptical of clinical hypnosis, attributing effects to placebo and relaxation. However, NAMS elevated it to Level I based on rigorous RCTs showing physiological frequency reductions that outpace standard relaxation/placebo controls.
    *   **Gate Result**: Not Singular.

## 6. Decision-relevant takeaways
*   **Prescribing Protocol**: Clinicians should actively refer patients with contraindications to HT (e.g., breast cancer survivors) to practitioners trained in clinical hypnosis and CBT specifically structured for menopause, rather than dismissing behavioral options as mere "relaxation" [→ C-001, C-002].

## 7. Gaps & next questions
1. Impact of weight loss (GLP-1s) on VMS [→ PERI-META-CONTROVERSIES].

## 8. Cross-stream dependencies
*   **Inputs from**: PERI-RX-NONHORMONAL (Alternative treatments).
*   **Outputs to**: PERI-DECISION (Treatment algorithms).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T21:10:00Z
— **Sources count**: Tier 1: 1, Tier 2: 0, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-RX-BEHAVIORAL
title: "Behavioral Interventions for Vasomotor Symptoms"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "CBT is Level I recommended for reducing VMS distress."
    category: C9
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "Future RCTs showing no superiority over control for distress."
    confidence_label: "[Verified · C9 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-002
    statement: "Clinical hypnosis reduces physiological VMS frequency (Level I evidence)."
    category: C5
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "Meta-analysis demonstrating hypnosis is purely placebo."
    confidence_label: "[Verified · C5 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-003
    statement: "CBT-I is a distinct, first-line intervention for menopause insomnia."
    category: C9
    rung: R5
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "Medical societies prioritizing pharmacology over CBT-I."
    confidence_label: "[Verified · C9 · R5 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: null

  - id: C-004
    statement: "Paced respiration is not recommended for VMS treatment due to lack of efficacy."
    category: C9
    rung: R4
    vantage: "consensus-statement"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "New high-power RCT showing efficacy of respiration."
    confidence_label: "[Verified · C9 · R4 · V=consensus-statement · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: null

sources:
  - id: S-001
    citation: "The 2023 nonhormone therapy position statement of NAMS"
    tier: 1
    authority: "named-position-statement"
    independence_chain_id: ICH-001
    incentive: "Society funding"
    url_or_doi: "NA"

independence_chains:
  - id: ICH-001
    terminal_upstream: "NAMS Consensus Panel"
    sources_in_chain: [S-001]
    corroboration_count: 0

controversies:
  - id: CTR-001
    statement: "Hypnosis vs Placebo Effect in VMS."
    classification: "disjoint-priors"
    singular_gate: { passed: false, resolving_experiment: "Double-blind sham-hypnosis RCT with physiological VMS tracking.", reason_unobtainable: "Difficult to design true sham behavioral therapy." }
    positions:
      - { name: "NAMS Recommendation", supporters: ["Menopause Societies"], strongest_evidence: [S-001], strongest_objection: "Mechanistic ambiguity." }

decisions_supported:
  - id: DEC-1
    decision: "Include CBT and Clinical Hypnosis as valid first-line referrals for VMS."
    contribution: "Provides highly effective pathways for patients refusing or contraindicated for pharmacology."
    relevant_claim_ids: [C-001, C-002]

gaps:
  - id: GAP-1
    statement: "Lifestyle/Diet modifications for VMS."
    reason: "out-of-scope"
    handoff_stream: PERI-CONTEXT

cross_stream:
  inputs_from: []
  outputs_to: [{ stream_id: PERI-DECISION, claim_ids: [C-001, C-002] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 1, tier2: 0, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
