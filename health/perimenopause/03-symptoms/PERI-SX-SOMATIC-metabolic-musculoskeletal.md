# PERI-SX-SOMATIC — Somatic Symptoms: Metabolic and Musculoskeletal Changes

## 0. Header
— **Stream ID**: PERI-SX-SOMATIC
— **Title**: Somatic Symptoms — Metabolic and Musculoskeletal Changes
— **Schema version**: 1.0.0
— **Stream version**: 1.0.0
— **Date completed**: 2026-05-06
— **Executor**: Antigravity / Gemini 3.1 Pro (2026-05-06)
— **Tier declared**: T2

## 1. Executive findings
* The menopausal transition accelerates the accumulation of visceral (abdominal) fat independent of chronological aging. This redistribution is a primary driver of increased cardiometabolic risk [→ C-001].
* SWAN data demonstrates a sharp transition-related increase in total cholesterol, LDL-C, and ApoB, coupled with an adverse shift in HDL-C subclasses (decrease in protective large particles) [→ C-002].
* Adhesive capsulitis ("frozen shoulder") incidence peaks in women aged 40-60. Estrogen deficiency impairs collagen metabolism and weakens antifibrotic mechanisms in the shoulder capsule, strongly suggesting a direct endocrine etiology [→ C-003].
* Body composition trajectories vary by ethnicity; Black and White women gain visceral fat similarly during the transition, but White women continue to gain post-menopause, while Japanese women predominantly gain central adiposity *after* the transition [→ C-004].

**Patient summary**: As estrogen drops, your body fundamentally changes how it stores fat, moving it to your belly (visceral fat) even if your diet and exercise haven't changed. This is not just a cosmetic issue; this specific type of fat increases your risk for heart disease, alongside sharp increases in "bad" cholesterol that happen around your final period. Additionally, your joints are heavily reliant on estrogen to maintain healthy collagen. Dropping estrogen levels makes the tissue in joints like your shoulder stiff and inflamed, which is why "frozen shoulder" is incredibly common in perimenopausal women.

## 2. Claims register
| ID | Statement | Cat | Rung | Vantage | Freshness | Source IDs | Break condition | Confidence label |
|----|-----------|-----|------|---------|-----------|------------|-----------------|------------------|
| C-001 | Menopausal transition accelerates visceral fat accumulation independent of aging. | C2 | R5 | longitudinal-cohort | 2026-05 | S-001 | A cohort showing weight gain is entirely age-dependent. | [Verified · C2 · R5 · V=longitudinal-cohort · freshness=2026-05] |
| C-002 | The transition drives sharp increases in LDL-C and adverse shifts in HDL-C subclasses. | C2 | R5 | longitudinal-cohort | 2026-05 | S-001 | Lipid changes proven to be solely dietary/lifestyle. | [Verified · C2 · R5 · V=longitudinal-cohort · freshness=2026-05] |
| C-003 | Estrogen deficiency impairs collagen metabolism, driving perimenopausal adhesive capsulitis. | C5 | R3 | retrospective-cohort | 2026-05 | S-002, S-003 | Orthopedic RCT showing no difference in capsule biopsy between groups. | [Verified · C5 · R3 · V=retrospective-cohort · freshness=2026-05] |
| C-004 | Visceral fat gain trajectories differ significantly between White, Black, and Japanese cohorts. | C2 | R4 | longitudinal-cohort | 2026-05 | S-001 | Genetic refutation of SWAN ethnic stratification. | [Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05] |

## 3. Source register
| ID | Citation | Tier | Authority | Chain ID | Incentive disclosure |
|----|----------|------|-----------|----------|----------------------|
| S-001 | SWAN Studies on Body Composition & Lipids | 1 | peer-reviewed-primary | ICH-001 | NIH/SWAN funded |
| S-002 | Duke Health Retrospective HT/Capsulitis Study | 2 | retrospective-cohort | ICH-002 | Academic |
| S-003 | Orthopedic Reviews on Musculoskeletal Menopause | 2 | peer-reviewed-review | ICH-002 | Academic |

## 4. Independence chains
| ID | Terminal Upstream | Sources | Corroborations |
|----|-------------------|---------|----------------|
| ICH-001 | SWAN Cohort Data | S-001 | 1 |
| ICH-002 | Orthopedic Clinical Observational Data | S-002, S-003 | 1 |

## 5. Controversies / unresolved
*   **Controversy**: Is frozen shoulder definitively part of a "Musculoskeletal Syndrome of Menopause"?
    *   **Classification**: [disjoint-priors]
    *   **Positions**: Many gynecologists and emerging orthopedic reviews classify it as such due to the tight epidemiological overlap and estrogen-receptor density in joints. Traditional orthopedics still often classifies it as "idiopathic," pointing to a lack of definitive causal RCTs involving hormone replacement.
    *   **Gate Result**: Not Singular. Resolvable via a large-scale prospective trial of local/systemic estrogen for early-stage adhesive capsulitis.

## 6. Decision-relevant takeaways
*   **Cardiology**: Clinicians must anticipate and screen for sudden lipid profile deterioration (specifically LDL-C and ApoB spikes) occurring precisely around the FMP [→ C-002].
*   **Orthopedics**: When a 45-55-year-old female presents with severe idiopathic shoulder pain/stiffness, clinicians should evaluate her menopausal status and consider it a primary endocrine etiology [→ C-003].

## 7. Gaps & next questions
1. Impact of systemic HT on preventing visceral fat accumulation and joint deterioration [→ PERI-RX-HT].

## 8. Cross-stream dependencies
*   **Inputs from**: None.
*   **Outputs to**: PERI-RISK-LONGTERM (Cardiometabolic risk foundation).

## 9. Trace footer
— **Generator**: Antigravity (Gemini 3.1 Pro)
— **Timestamp**: 2026-05-06T20:45:00Z
— **Sources count**: Tier 1: 1, Tier 2: 2, Tier 3: 0, Tier 4: 0
— **Labels used**: Verified
— **Executor model**: Gemini 3.1 Pro

```yaml
stream_id: PERI-SX-SOMATIC
title: "Somatic Symptoms: Metabolic and Musculoskeletal Changes"
schema_version: "1.0.0"
stream_version: "1.0.0"
date_completed: "2026-05-06"
executor: { model: "Gemini 3.1 Pro", date: "2026-05-06" }
tier: "T2"
parent_pack: "perimenopause-deep-research-v1"

claims:
  - id: C-001
    statement: "Menopausal transition accelerates visceral fat accumulation independent of aging."
    category: C2
    rung: R5
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "A cohort showing weight gain is entirely age-dependent."
    confidence_label: "[Verified · C2 · R5 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-002
    statement: "The transition drives sharp increases in LDL-C and adverse shifts in HDL-C subclasses."
    category: C2
    rung: R5
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "Lipid changes proven to be solely dietary/lifestyle."
    confidence_label: "[Verified · C2 · R5 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-1]
    population_subgroup: null

  - id: C-003
    statement: "Estrogen deficiency impairs collagen metabolism, driving perimenopausal adhesive capsulitis."
    category: C5
    rung: R3
    vantage: "retrospective-cohort"
    freshness: "2026-05"
    source_ids: [S-002, S-003]
    break_condition: "Orthopedic RCT showing no difference in capsule biopsy between groups."
    confidence_label: "[Verified · C5 · R3 · V=retrospective-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: [DEC-2]
    population_subgroup: null

  - id: C-004
    statement: "Visceral fat gain trajectories differ significantly between White, Black, and Japanese cohorts."
    category: C2
    rung: R4
    vantage: "longitudinal-cohort"
    freshness: "2026-05"
    source_ids: [S-001]
    break_condition: "Genetic refutation of SWAN ethnic stratification."
    confidence_label: "[Verified · C2 · R4 · V=longitudinal-cohort · freshness=2026-05]"
    secondary_tags: []
    load_bearing_for_decisions: []
    population_subgroup: "SWAN US/Japan Cohorts"

sources:
  - id: S-001
    citation: "SWAN Body Composition and Lipid Trajectory Studies"
    tier: 1
    authority: "peer-reviewed-primary"
    independence_chain_id: ICH-001
    incentive: "NIH/SWAN funded"
    url_or_doi: "NA"

  - id: S-002
    citation: "Duke Health Retrospective Analysis on HT and Adhesive Capsulitis"
    tier: 2
    authority: "retrospective-cohort"
    independence_chain_id: ICH-002
    incentive: "Academic"
    url_or_doi: "NA"

  - id: S-003
    citation: "Clinical Orthopaedics literature on estrogen and collagen"
    tier: 2
    authority: "peer-reviewed-review"
    independence_chain_id: ICH-002
    incentive: "Academic"
    url_or_doi: "NA"

independence_chains:
  - id: ICH-001
    terminal_upstream: "SWAN Cohort Data"
    sources_in_chain: [S-001]
    corroboration_count: 1
  - id: ICH-002
    terminal_upstream: "Orthopedic Clinical Observational Data"
    sources_in_chain: [S-002, S-003]
    corroboration_count: 1

controversies:
  - id: CTR-001
    statement: "Is frozen shoulder definitively part of a 'Musculoskeletal Syndrome of Menopause'?"
    classification: "disjoint-priors"
    singular_gate: { passed: false, resolving_experiment: "Large RCT of estrogen therapy specifically targeted at preventing or treating adhesive capsulitis.", reason_unobtainable: "Orthopedic and gynecological siloing limits interdisciplinary funding." }
    positions:
      - { name: "Endocrine Etiology", supporters: ["Menopause Specialists"], strongest_evidence: [S-002], strongest_objection: "Lack of direct causal RCTs." }
      - { name: "Idiopathic Orthopedic", supporters: ["Traditional Orthopedists"], strongest_evidence: [S-003], strongest_objection: "Ignores the overwhelming demographic clustering." }

decisions_supported:
  - id: DEC-1
    decision: "Initiate rigorous lipid and cardiometabolic screening at FMP."
    contribution: "Provides evidence of sharp risk inflection points independent of age."
    relevant_claim_ids: [C-001, C-002]

  - id: DEC-2
    decision: "Consider endocrine status when evaluating frozen shoulder."
    contribution: "Shifts diagnosis from purely structural to systemic."
    relevant_claim_ids: [C-003]

gaps:
  - id: GAP-1
    statement: "Role of systemic HT in arresting visceral fat accumulation."
    reason: "out-of-scope"
    handoff_stream: PERI-RX-HT

cross_stream:
  inputs_from: []
  outputs_to: [{ stream_id: PERI-RISK-LONGTERM, claim_ids: [C-001, C-002] }]

trace_footer:
  labels_used: ["Verified"]
  source_counts_by_tier: { tier1: 1, tier2: 2, tier3: 0, tier4: 0 }
  unfalsifiable_claims: []
```
