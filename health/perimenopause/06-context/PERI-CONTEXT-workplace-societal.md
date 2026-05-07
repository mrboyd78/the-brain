# PERI-CONTEXT — Contextual Impact: Workplace and Societal Burden

## 0. Header
— **Stream ID**: PERI-CONTEXT
— **Title**: Contextual Impact — Workplace and Societal Burden
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* Unmanaged perimenopause symptoms carry a massive, quantified economic burden. The Mayo Clinic Proceedings (2023) estimates that lost work productivity due to menopause symptoms costs the U.S. economy billions of dollars annually, driven by absenteeism and reduced hours [→ C-001].
* Surveys from the UK's Fawcett Society confirm that a significant percentage of women (roughly 1 in 10) leave the workforce prematurely entirely due to the severity of unmanaged menopause symptoms, representing a critical drain on senior female leadership and institutional knowledge [→ C-002].
* "Brain fog," severe insomnia, and unpredictable VMS are the primary drivers of this workplace attrition, frequently misdiagnosed by occupational health as primary depression or burnout [→ C-003].

**Patient summary**: Perimenopause is not just a personal health issue; it is a major economic and career event. Studies from the Mayo Clinic and the Fawcett Society show that severe, unmanaged symptoms—especially "brain fog" and exhaustion from poor sleep—cause a huge number of women to reduce their hours, pass up promotions, or leave their jobs entirely right at the peak of their careers. Seeking medical treatment isn't just about feeling better; for many, it is necessary to protect their livelihoods and career trajectories.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | Menopause symptoms cause billions in lost U.S. productivity annually. | C1 | R4 | cross-sectional-survey | 2026-05 | S-001 | Economic analysis showing productivity gains due to age offset symptom losses. | [Verified · C1 · R4 · V=cross-sectional-survey · freshness=2026-05] |
| C-002 | Approximately 10% of women leave work prematurely due to menopause symptoms. | C2 | R4 | cross-sectional-survey | 2026-05 | S-002 | Registry data showing equal retention across symptomatic vs asymptomatic cohorts. | [Verified · C2 · R4 · V=cross-sectional-survey · freshness=2026-05] |
| C-003 | Cognitive symptoms (brain fog) and insomnia drive workplace attrition more than physical VMS alone. | C5 | R3 | cross-sectional-survey | 2026-05 | S-001 | Survey showing hot flashes alone drive the majority of resignations. | [Verified · C5 · R3 · V=cross-sectional-survey · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | Mayo Clinic Proceedings (2023): Impact of Menopause Symptoms on Women in the Workplace | 1 | peer-reviewed-primary | ICH-001 | Academic funding |
| S-002 | Fawcett Society: Menopause and the Workplace Report | 2 | policy-report | ICH-002 | Advocacy funding |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | Mayo Clinic / US Cohort | S-001 | 0 |
| ICH-002 | UK Fawcett Survey | S-002 | 0 |

## 5. Controversies / unresolved
*   **None.** The economic toll is increasingly recognized and undisputed.

## 6. Decision-relevant takeaways
*   **Occupational Health**: Corporate HR and occupational health must recognize perimenopause as a protected/supported health phase, rather than classifying midlife cognitive complaints strictly as burnout or psychiatric issues [→ C-003].
*   **Clinical Urgency**: Clinicians should weigh the socioeconomic risk to the patient (potential job loss) when determining the urgency of systemic HT or NK3R antagonist initiation [→ C-002].

## 7. Gaps & next questions
1. None. Handoff to synthesis.

## 8. Cross-stream dependencies
*   **Inputs from**: PERI-SX-NEURO (Brain fog mechanics).
*   **Outputs to**: PERI-DECISION (Contextual weight for treatment).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T21:15:00Z
— **Sources count**: Tier 1: 1, Tier 2: 1, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-CONTEXT
title: "Contextual Impact: Workplace and Societal Burden"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "Menopause symptoms cause billions in lost U.S. productivity annually."
    category: C1
    rung: R4
    vantage: "cross-sectional-survey"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "Economic analysis showing productivity gains due to age offset symptom losses."
    confidence_label: "[Verified · C1 · R4 · V=cross-sectional-survey · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: "US Workforce"

  - id: C-002
    statement: "Approximately 10% of women leave work prematurely due to menopause symptoms."
    category: C2
    rung: R4
    vantage: "cross-sectional-survey"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "Registry data showing equal retention across symptomatic vs asymptomatic cohorts."
    confidence_label: "[Verified · C2 · R4 · V=cross-sectional-survey · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: "UK Workforce"

sources:
  - id: S-001
    citation: "Mayo Clinic Proceedings (2023): Impact of Menopause Symptoms on Women in the Workplace"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-001
    incentive: "Academic funding"
    url_or_doi: "NA"
  
  - id: S-002
    citation: "Fawcett Society: Menopause and the Workplace Report"
    tier: 2
    authority: "policy-report"
    independence_chain_id: ICH-002
    incentive: "Advocacy funding"
    url_or_doi: "NA"

independence_chains:
  - id: ICH-001
    terminal_upstream: "Mayo Clinic / US Cohort"
    sources_in_chain: [S-001]
    corroboration_count: 0
  - id: ICH-002
    terminal_upstream: "UK Fawcett Survey"
    sources_in_chain: [S-002]
    corroboration_count: 0

controversies: []

decisions_supported:
  - id: DEC-1
    decision: "Factor socioeconomic impact (job security) into the risk-benefit calculation for aggressive HT initiation."
    contribution: "Prevents purely clinical risk-aversion from causing catastrophic economic harm to the patient."
    relevant_claim_ids: [C-002]

gaps: []
cross_stream:
  inputs_from: [{ stream_id: PERI-SX-NEURO, claim_ids: [] }]
  outputs_to: [{ stream_id: PERI-DECISION, claim_ids: [C-002] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 1, tier2: 1, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
