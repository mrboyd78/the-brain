# PERI-EPI — Epidemiology, Demographics, and Racial/Ethnic Variation in Perimenopause

## 0. Header
— **Stream ID**: PERI-EPI
— **Title**: Epidemiology, Demographics, and Racial/Ethnic Variation in Perimenopause
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* The median age of natural menopause (FMP) is 51.3 years, with the transition phase lasting a median of 3.8 years [→ C-001].
* Vasomotor symptoms (VMS) last significantly longer than historically assumed: the median total duration is 7.4 years, with a median 4.5 years of persistence post-FMP [→ C-002].
* Total VMS duration exhibits profound racial/ethnic variation: African American women experience the longest duration (median 10.1 years), compared to Hispanic (~8.9 years) and non-Hispanic white (~6.5 years) women [→ C-003].
* Early menopause (loss of ovarian function before age 45) affects approximately 12.2% of women globally [→ C-004].
* Primary Ovarian Insufficiency (POI, <40 years) affects approximately 3.7% globally, challenging older estimates of 1% [→ C-005].

**Patient summary**: The average age for a final menstrual period is 51, but the symptomatic transition phase usually starts around age 47. Hot flashes and night sweats last much longer than people used to think: the average is over 7 years total, and they often continue for 4.5 years after your final period. Your experience can vary greatly based on your race and ethnicity; for example, Black women experience hot flashes for 10 years on average. About 1 in 8 women will experience menopause early (before 45).

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | Median age at natural menopause is 51.3 years; transition length is 3.8 years. | C1 | R4 | longitudinal-cohort | 2026-05 | S-001 | New global meta-analysis displacing MWHS norms. | [Verified · C1 · R4 · V=longitudinal-cohort · freshness=2026-05] |
| C-002 | Median total VMS duration is 7.4 years; post-FMP persistence is 4.5 years. | C2 | R5 | longitudinal-cohort | 2026-05 | S-002 | A larger cohort refuting SWAN duration measures. | [Verified · C2 · R5 · V=longitudinal-cohort · freshness=2026-05] |
| C-003 | African American women experience median VMS duration of 10.1 years (vs 6.5 white). | C2 | R4 | longitudinal-cohort | 2026-05 | S-002 | A study demonstrating this is confounded by measurement instrument. | [Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05] |
| C-004 | Early menopause (<45) prevalence is approximately 12.2% globally. | C1 | R4 | meta-analysis | 2026-05 | S-003 | Global re-baselining of early menopause datasets. | [Verified · C1 · R4 · V=meta-analysis · freshness=2026-05] |
| C-005 | Primary Ovarian Insufficiency (<40) prevalence is approximately 3.7%. | C1 | R4 | meta-analysis | 2026-05 | S-003 | Methodological refutation of the 2019 Golezar meta-analysis. | [Verified · C1 · R4 · V=meta-analysis · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | McKinlay et al., Maturitas, 1992 (MWHS) | 1 | peer-reviewed-primary | ICH-001 | NIH funded |
| S-002 | Avis et al., JAMA Intern Med, 2015; 175(4):531-539 | 1 | peer-reviewed-primary | ICH-002 | NIH/SWAN funded |
| S-003 | Golezar et al., Climacteric, 2019; 22(4):403-411 | 1 | high-quality-sys-review | ICH-003 | Academic/None disclosed |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | Massachusetts Women's Health Study (MWHS) | S-001 | 1 |
| ICH-002 | SWAN longitudinal cohort | S-002 | 1 |
| ICH-003 | Golezar 2019 Global POI Meta-Analysis | S-003 | 1 |

## 5. Controversies / unresolved
*   **Controversy**: The true global prevalence of POI (1% vs 3.7%).
    *   **Classification**: [correlated-sources]
    *   **Positions**: Older textbooks cite 1% based on narrow clinic-ascertained denominators; recent global meta-analyses (Golezar) cite 3.7% encompassing community denominators in lower-HDI regions.
    *   **Gate Result**: Not Singular. Dispute is over sampling denominators (clinic vs population).

## 6. Decision-relevant takeaways
*   **Counseling**: Clinicians must stop counseling patients that hot flashes will "last a couple of years" and update counseling to reflect a median 7.4-year duration, adjusting for the patient's race/ethnicity [→ C-002, C-003].
*   **Screening**: Recognize that 1 in 8 women will experience menopause before 45, elevating their risk for cardiovascular and bone density impacts requiring earlier intervention [→ C-004].

## 7. Gaps & next questions
1. Clinical implications and symptom burden (VMS mechanisms) [→ PERI-SX-VMS].
2. Long-term risk modification for early menopause patients [→ PERI-RISK-LONGTERM].

## 8. Cross-stream dependencies
*   **Inputs from**: PERI-DEF (Definitions of transition staging).
*   **Outputs to**: PERI-CONTEXT (Disparities in care context), PERI-SX-VMS (VMS baseline expectations).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T20:20:00Z
— **Sources count**: Tier 1: 3, Tier 2: 0, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-EPI
title: "Epidemiology, Demographics, and Racial/Ethnic Variation in Perimenopause"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "Median age at natural menopause is 51.3 years; transition length is 3.8 years."
    category: C1
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "New global meta-analysis displacing MWHS norms."
    confidence_label: "[Verified · C1 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: null

  - id: C-002
    statement: "Median total VMS duration is 7.4 years; post-FMP persistence is 4.5 years."
    category: C2
    rung: R5
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "A larger cohort refuting SWAN duration measures."
    confidence_label: "[Verified · C2 · R5 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-003
    statement: "African American women experience median VMS duration of 10.1 years (vs 6.5 white)."
    category: C2
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-002]
    break_condition: "A study demonstrating this is confounded by measurement instrument."
    confidence_label: "[Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: "SWAN US Cohort"

  - id: C-004
    statement: "Early menopause (<45) prevalence is approximately 12.2% globally."
    category: C1
    rung: R4
    vantage: "meta-analysis"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "Global re-baselining of early menopause datasets."
    confidence_label: "[Verified · C1 · R4 · V=meta-analysis · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-005
    statement: "Primary Ovarian Insufficiency (<40) prevalence is approximately 3.7%."
    category: C1
    rung: R4
    vantage: "meta-analysis"
    freshness: "2026-05"
    source_ids: [S-003]
    break_condition: "Methodological refutation of the 2019 Golezar meta-analysis."
    confidence_label: "[Verified · C1 · R4 · V=meta-analysis · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

sources:
  - id: S-001
    citation: "McKinlay et al., Maturitas, 1992; 14(2):103-115"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-001
    incentive: "NIH funded"
    url_or_doi: "10.1016/0378-5122(92)90003-M"

  - id: S-002
    citation: "Avis et al., JAMA Intern Med, 2015; 175(4):531-539"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-002
    incentive: "NIH/SWAN funded"
    url_or_doi: "10.1001/jamainternmed.2014.8063"

  - id: S-003
    citation: "Golezar et al., Climacteric, 2019; 22(4):403-411"
    tier: 1
    authority: "high-quality-sys-review"
    independence_chain_id: ICH-003
    incentive: "None disclosed"
    url_or_doi: "10.1080/13697137.2019.1574738"

independence_chains:
  - id: ICH-001
    terminal_upstream: "Massachusetts Women's Health Study (MWHS)"
    sources_in_chain: [S-001]
    corroboration_count: 1
  
  - id: ICH-002
    terminal_upstream: "SWAN longitudinal cohort"
    sources_in_chain: [S-002]
    corroboration_count: 1

  - id: ICH-003
    terminal_upstream: "Golezar 2019 Global POI Meta-Analysis"
    sources_in_chain: [S-003]
    corroboration_count: 1

controversies:
  - id: CTR-001
    statement: "The true global prevalence of POI (1% vs 3.7%)."
    classification: "correlated-sources"
    singular_gate: { passed: false, resolving_experiment: "Large-scale highly standardized global prevalence registry", reason_unobtainable: "Requires massive cross-national funding and aligned criteria." }
    positions:
      - { name: "Clinic-ascertained denominator (1%)", supporters: ["Historical textbooks"], strongest_evidence: [S-003], strongest_objection: "Under-samples populations without access to specialized fertility clinics." }
      - { name: "Community-ascertained denominator (3.7%)", supporters: ["Recent meta-analyses"], strongest_evidence: [S-003], strongest_objection: "May include some methodologically weak component studies." }

decisions_supported:
  - id: DEC-1
    decision: "Counsel patients on a 7-10 year VMS horizon, adjusting for ethnicity."
    contribution: "Overturns the historical 2-3 year assumption, establishing SWAN baselines."
    relevant_claim_ids: [C-002, C-003]

  - id: DEC-2
    decision: "Heighten screening for long-term health risks in women reaching menopause before 45."
    contribution: "Establishes that 1 in 8 women face this accelerated timeline."
    relevant_claim_ids: [C-004, C-005]

gaps:
  - id: GAP-1
    statement: "Mechanisms causing the ethnic variation in VMS duration."
    reason: "out-of-scope"
    handoff_stream: PERI-SX-VMS

cross_stream:
  inputs_from: [{ stream_id: PERI-DEF, claim_ids: [C-001] }]
  outputs_to: [{ stream_id: PERI-CONTEXT, claim_ids: [C-003] }, { stream_id: PERI-SX-VMS, claim_ids: [C-002, C-003] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 3, tier2: 0, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
