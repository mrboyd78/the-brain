# matrix-architect — Systems Formalization Engine

A systems-formalization plugin that turns ideas, operations, and products into coherent architectures with explicit rules, incentives, constraints, and failure tolerance — following the event-bus, hook, DAG plugin system.

---

## Differentiation from `the-architect`

| | `the-architect` | `matrix-architect` |
|---|---|---|
| **Domain** | Software/code architecture | System/business/org architecture |
| **Focus** | Target-state design, boundaries, interfaces, migration, handoffs for `the-coder` | Rules, incentives, constraints, governance, failure containment, formalization thresholds |
| **Output** | Design packets, invariants, interface contracts, migration plans | Structural maps, rule sets, incentive audits, constraint grids, governance layers |
| **Consumers** | `the-coder`, `asimov` | All plugins dealing with product, org, marketplace, workflow, platform design |

They are complementary: `the-architect` designs the code; `matrix-architect` designs the operating system around it.

---

## Operating Principle

> Design the minimum structure required for coherence, durability, and leverage.

**Tagline:** *"Structure is what survives contact with reality."*

---

## Two-Layer Design

**Layer 1 — Decomposition:** Break messy situations into structural components — actors, goals, flows, permissions, incentives, dependencies, chokepoints, feedback loops, failure points. Make the system legible in structural terms, not just narrative terms.

**Layer 2 — Formalization:** Design rules, constraints, incentives, interfaces, and control mechanisms that make the structure durable. Determine what should be codified now versus what should remain fluid. Engineer governance and failure tolerance.

The tension: decomposition without formalization is just a diagram. Formalization without decomposition is bureaucracy imposed on a system nobody understands. matrix-architect does both.

---

## Pipeline (7 stages)

### 1. Decompose
Structural decomposition: break the situation into actors, goals, flows, permissions, incentives, dependencies, chokepoints, feedback loops, failure points.

- Map all actors and their roles, motivations, and power
- Trace all flows (value, information, decisions, money, permissions)
- Identify all dependencies (explicit and implicit)
- Surface chokepoints where flow concentrates
- Detect feedback loops (reinforcing and balancing)
- Classify system archetype (hierarchy, marketplace, platform, pipeline, hub-and-spoke, mesh, hybrid)

**Agent:** The Surveyor
**Polymath models:** `systems-thinking`, `stakeholder-map`, `process-mapping`

**Output:**
- `structural-map.json` — actors, flows, dependencies, chokepoints, feedback loops, archetype

---

### 2. Identify
Load-bearing analysis: find the small number of elements that hold the system together.

- Score each structural element by how many flows pass through it
- Classify as load-bearing (removal = collapse), supporting (removal = degradation), or decorative (removal = cosmetic)
- Map dependency fan-out: which elements have the most downstream dependents?
- Detect hidden load-bearing elements that aren't labeled as important but carry weight
- Calculate bus factor per load-bearing element
- Assess concentration risk: too much weight on too few elements?

**Agent:** The Surveyor
**Polymath models:** `graph-theory`, `dependency-analysis`, `critical-path`

**Output:**
- `load-bearing-analysis.json` — classified elements with criticality scores, bus factor, concentration risk

---

### 3. Rule
Rule design: determine what rules need to exist and what rules are broken.

- Audit existing rules: what is currently enforced (formally or informally)?
- Identify missing rules: where does ambiguity cause drift, conflict, or failure?
- Identify rigid rules: where does excessive formality create friction without value?
- Classify rules: default (assumed unless overridden), optional (available but not required), forbidden (never permitted)
- Detect rule conflicts: where do two rules contradict?
- Assess rule enforcement: is the rule enforced by structure, by culture, or by hope?
- Distinguish productive ambiguity from dangerous ambiguity

**Agent:** The Legislator
**Polymath models:** `game-theory`, `institutional-design`, `mechanism-design`

**Output:**
- `rule-audit.json` — existing/missing/rigid/conflicting rules with classification and enforcement method
- `rule-design.json` — proposed rule set with rationale and enforcement mechanism

---

### 4. Align
Incentive alignment audit: what behavior does this structure actually reward?

- Map stated goals vs. actual incentive gradients
- Identify perverse incentives: where rational behavior under current rules undermines goals
- Detect gaming vectors: where the rules can be exploited to produce bad outcomes
- Assess behavioral divergence: gap between intended and actual behavior
- Find alignment opportunities: changes that make the desired behavior the easiest behavior
- Model second-order effects: how do incentive changes cascade through the system?

**Agent:** The Behaviorist
**Polymath models:** `behavioral-economics`, `game-theory`, `principal-agent`

**Output:**
- `incentive-audit.json` — stated goals vs. actual incentive gradients, perverse incentives, gaming vectors
- `alignment-recommendations.json` — proposed changes with expected behavioral impact

---

### 5. Constrain
Constraint engineering: separate productive constraint from needless rigidity.

- Classify existing constraints: order-creating, friction-creating, chaos-preventing, or arbitrary
- Identify missing constraints: where does absence of constraint invite chaos or abuse?
- Identify upstream vs. downstream constraints: should this constraint be moved earlier?
- Design constraint hierarchy: hard (never violated), soft (violated with escalation), advisory (recommended)
- Assess constraint cost: what does each constraint cost in speed, flexibility, or morale?
- Check for constraint contradiction: two constraints that cannot both be satisfied

**Agent:** The Legislator
**Polymath models:** `constraint-satisfaction`, `optimization-theory`, `institutional-design`

**Output:**
- `constraint-grid.json` — classified constraints with cost, hierarchy, and contradiction detection
- `constraint-recommendations.json` — add/remove/move/reclassify proposals

---

### 6. Harden
Failure containment + simulation: how does this system break, and what should we do about it?

- Map failure modes: what breaks, how, and what cascades
- Classify failure severity: cosmetic, degrading, critical, existential
- Design graceful degradation: what should the system do when parts fail?
- Identify redundancy needs: what must never go down?
- Design monitoring: what signals indicate impending failure?
- Stress-test against: misuse, scale, edge cases, drift, adversarial behavior
- Check equilibrium tendency: does the system naturally tend toward order or disorder?

**Agent:** The Stress-Tester
**Polymath models:** `resilience-engineering`, `failure-mode-analysis`, `chaos-engineering`

**Output:**
- `failure-map.json` — failure modes with severity, cascade paths, detection methods
- `hardening-plan.json` — redundancy, monitoring, graceful degradation, equilibrium assessment

---

### 7. Formalize
Governance design + formalization threshold: what should be codified now, what stays fluid?

- Design governance layer: decision rights, escalation paths, accountability, oversight
- Determine formalization threshold per element: ready for policy? tooling? documentation? automation?
- Detect premature formalization: attempting to codify what isn't understood yet
- Detect delayed formalization: allowing entropy in areas that need structure now
- Design revision cycle: when should this architecture be re-evaluated?
- Produce minimum viable architecture: the least structure required for coherence

**Agent:** The Stress-Tester
**Polymath models:** `governance-design`, `organizational-theory`, `decision-theory`

**Output:**
- `governance-layer.json` — decision rights, escalation paths, accountability, permissions
- `formalization-plan.json` — per-element formalization readiness with timing recommendation
- `revision-schedule.json` — architecture review triggers and timeline
- `architecture-summary.md` — human-readable architecture document

---

## Agents (4)

### The Surveyor
Meticulous, structural, sees connections. Maps the invisible architecture that holds things together. Cares about completeness and accuracy, not aesthetics.

**Stages:** Decompose, Identify
**Voice:** Precise, structural, visual. Uses terms like "this element carries 4 downstream dependencies" not "this seems important."

### The Legislator
Rule-obsessed, sees both the letter and spirit. Designs rules that create order without suffocating emergence. Understands that the best rules make the desired behavior the easiest behavior.

**Stages:** Rule, Constrain
**Voice:** Authoritative but flexible. Distinguishes between "must," "should," and "may." Always asks "how is this enforced?"

### The Behaviorist
Cynical about intentions, attentive to actual behavior. Assumes people will follow incentive gradients, not mission statements. Finds where the structure accidentally rewards the wrong behavior.

**Stages:** Align
**Voice:** Skeptical, empirical. Uses phrases like "the structure rewards X regardless of stated preference for Y."

### The Stress-Tester
Destructive by nature, constructive by purpose. Tries to break the architecture to find what needs reinforcement. Knows that systems optimized for ideal conditions fail first.

**Stages:** Harden, Formalize
**Voice:** Adversarial, scenario-driven. "What happens when [bad thing] occurs? The current design does nothing."

---

## Scoring Rubric: 9-Axis Architectural Integrity Score

### Layer 1 — Structural Clarity (3 axes)

| Axis | Weight | Description |
|------|--------|-------------|
| Decomposition Completeness | ×2 | All actors, flows, dependencies, chokepoints mapped |
| Load-Bearing Identification | ×1 | Critical structural elements identified with bus factor |
| Dependency Transparency | ×2 | Hidden dependencies surfaced, concentration risk assessed |

### Layer 2 — Architectural Durability (6 axes)

| Axis | Weight | Description |
|------|--------|-------------|
| Rule Coherence | ×2 | Rules consistent, complete, non-contradictory, enforceable |
| Incentive Alignment | ×2 | Behavior shaped toward goals, perverse incentives eliminated |
| Constraint Quality | ×1 | Productive constraints without arbitrary friction |
| Failure Tolerance | ×2 | Graceful degradation designed, existential failures contained |
| Formalization Readiness | ×1 | Right things codified at right time, no premature or delayed formalization |
| Governance Clarity | ×1 | Decision rights, escalation, oversight designed |

### Grades
| Grade | Name | Score |
|-------|------|-------|
| **S** | Operating System | 90-100 |
| **A** | Durable Architecture | 75-89 |
| **B** | Workable Structure | 60-74 |
| **C** | Fragile Scaffold | 40-59 |
| **D** | Duct Tape | 0-39 |

### Named Warnings
| Warning | Condition |
|---------|-----------|
| **Invisible Load** | Load-bearing elements unidentified (Axis 2 < 4) |
| **Perverse Machine** | Incentive alignment < 4 (structure rewards wrong behavior) |
| **Rule Vacuum** | Rule coherence < 3 (system operates on hope) |
| **Paper Tiger** | Rules exist but enforcement = "hope" on > 50% |
| **Overengineered** | Constraint quality < 4 due to excess friction |
| **House of Cards** | Failure tolerance < 3 (no degradation design) |
| **Premature Concrete** | Formalization readiness < 4 due to early codification |
| **Entropy Engine** | Equilibrium tendency = disorder with no governance |

### Layer Imbalance Detection
- **Mapped But Fragile:** Layer 1 avg ≥ 7, Layer 2 avg ≤ 4 — system is understood but not architected
- **Architected Blind:** Layer 1 avg ≤ 4, Layer 2 avg ≥ 7 — rules imposed on a system nobody mapped

---

## Key Concepts

1. **Structural Legibility** — Making invisible architecture visible in structural terms
2. **Load-Bearing Elements** — The small number of things that hold everything together; removal = collapse
3. **Rule Coherence** — Rules that are consistent, complete, enforceable, and not contradictory
4. **Incentive Gradient** — The direction behavior naturally flows under current structure
5. **Constraint Spectrum** — From productive order through neutral to arbitrary friction
6. **Formalization Threshold** — The point at which codification becomes necessary vs. premature
7. **Architecture Debt** — Unformalized critical structures that accumulate risk
8. **Equilibrium Tendency** — Whether a system naturally tends toward order or disorder without intervention
9. **Governance Clarity** — Who decides what, when, and under what rules
10. **Minimum Viable Architecture** — The least structure required for coherence, durability, and leverage
11. **Structural Entropy** — Architecture degrades over time without maintenance; formalization slows but doesn't stop decay
12. **Behavioral Divergence** — Gap between intended behavior (what rules say) and actual behavior (what incentives produce)

---

## Commands (9)

| Command | Purpose | Stages |
|---------|---------|--------|
| `/matrix-architect` | Full pipeline | All 7 stages |
| `/matrix-architect-decompose` | Structural decomposition | Decompose |
| `/matrix-architect-load-bearing` | Load-bearing analysis | Identify |
| `/matrix-architect-rule` | Rule design | Rule |
| `/matrix-architect-align` | Incentive alignment audit | Align |
| `/matrix-architect-constrain` | Constraint engineering | Constrain |
| `/matrix-architect-harden` | Failure containment | Harden |
| `/matrix-architect-formalize` | Governance + formalization | Formalize |
| `/matrix-architect-audit` | Architecture health audit | Scoring across all stages |

### Command Arguments (full pipeline)
| Arg | Type | Default | Description |
|-----|------|---------|-------------|
| `--domain` | enum | auto | Domain: product, org, marketplace, workflow, business-model, governance, platform |
| `--depth` | enum | standard | Depth: shallow (decompose only), standard (full), deep (+ stress simulation) |
| `--mode` | enum | full | Mode: system-map, rule-engine, incentive-lens, constraint-grid, fault-tolerance, governance-layer, formalize-now |
| `--focus` | string | all | Focus on specific subsystem or actor |
| `--adversarial` | flag | false | Include adversarial stress-testing (misuse, gaming, exploitation) |

---

## References (8)

| File | Content |
|------|---------|
| `structural-decomposition.md` | Actor/flow/dependency/chokepoint taxonomy, system archetypes, decomposition methodology |
| `load-bearing-patterns.md` | Load-bearing classification, concentration risk, bus factor, hidden dependency patterns |
| `rule-design-patterns.md` | Rule types, missing/rigid/productive classification, enforcement methods, conflict resolution |
| `incentive-alignment.md` | Incentive audit methodology, perverse incentive catalog, behavioral divergence patterns |
| `constraint-engineering.md` | Constraint classification, upstream/downstream analysis, hierarchy design, cost assessment |
| `failure-containment.md` | Failure modes, cascade paths, graceful degradation, redundancy, monitoring, equilibrium |
| `architecture-scoring-rubric.md` | 9-axis scoring, grades, named warnings, layer imbalance, tracking |
| `polymath-coordination.md` | Model compositions per pipeline stage |

---

## Event Bus Integration

### Emits (13)

| Event | When | Payload |
|-------|------|---------|
| `matrix-architect.decomposition.completed` | After Decompose | { actors_count, flows_count, dependencies_count, archetype } |
| `matrix-architect.load-bearing.identified` | After Identify | { load_bearing_count, concentration_risk, bus_factor_min } |
| `matrix-architect.rules.designed` | After Rule | { rules_count, missing_count, rigid_count, conflicts_count } |
| `matrix-architect.incentive-misalignment.detected` | Per perverse incentive | { behavior, intended_goal, actual_outcome, severity } |
| `matrix-architect.constraints.engineered` | After Constrain | { productive_count, arbitrary_count, missing_count } |
| `matrix-architect.failure-mode.identified` | Per failure mode | { mode, severity, cascade_path, containment } |
| `matrix-architect.governance.designed` | After Formalize | { decision_rights_count, escalation_paths, oversight } |
| `matrix-architect.formalization.recommended` | Per item ready | { element, readiness, method, urgency } |
| `matrix-architect.architecture-debt.flagged` | If debt > threshold | { debt_score, items, severity } |
| `matrix-architect.equilibrium.assessed` | After Harden | { tendency, stability_score, intervention_required } |
| `matrix-architect.integrity-score.published` | After scoring | { score, grade, layer1_avg, layer2_avg, warnings } |
| `matrix-architect.session.completed` | End of pipeline | { stages_run, artifacts, duration_ms } |
| `matrix-architect.thinker-intake.ready` | After pipeline | { patterns, rules, constraints, learnings } |

### Subscribes (14)

| Event | Source | Handler | Purpose |
|-------|--------|---------|---------|
| `archimedes.system-model.updated` | archimedes | on-system-model-updated.ts | System structure feeds decomposition |
| `archimedes.leverage-points.ranked` | archimedes | on-leverage-points-ranked.ts | Leverage points inform load-bearing analysis |
| `archimedes.invariants.identified` | archimedes | on-invariants-identified.ts | System invariants feed rule design |
| `asimov.constraints.defined` | asimov | on-constraints-defined.ts | Safety constraints feed constraint engineering |
| `asimov.boundary.mapped` | asimov | on-boundary-mapped.ts | Operating envelope informs constraint grid |
| `flawed-consensus.opportunity-map.completed` | flawed-consensus | on-opportunity-map-completed.ts | Broken assumptions reveal structural weaknesses |
| `rube-goldberg.chain.traced` | rube-goldberg | on-chain-traced.ts | Execution chains reveal hidden dependencies |
| `rube-goldberg.spof.detected` | rube-goldberg | on-spof-detected.ts | SPOFs reveal structural fragility |
| `ad-infinitum.design.proposed` | ad-infinitum | on-flywheel-proposed.ts | Flywheel designs need architectural grounding |
| `eccentric-billionaire.dare.structured` | eccentric-billionaire | on-dare-structured.ts | Dares need architectural feasibility assessment |
| `goal-digger.priorities.ranked` | goal-digger | on-priorities-ranked.ts | Priorities inform formalization order |
| `amicus.impact.assessed` | amicus | on-stakeholder-impact.ts | Stakeholder impacts inform governance design |
| `elliot.findings.published` | elliot | on-adversarial-findings.ts | Adversarial findings reveal exploitable structural gaps |
| `the-architect.design.completed` | the-architect | on-architect-design.ts | Software architecture feeds system architecture analysis |

---

## Collaboration

| Plugin | How |
|--------|-----|
| `archimedes` | System model feeds decomposition; leverage points inform load-bearing; invariants feed rule design |
| `asimov` | Safety constraints and operating envelope feed constraint engineering |
| `flawed-consensus` | Broken assumptions reveal where architecture is built on false foundations |
| `rube-goldberg` | Execution chains reveal hidden structural dependencies; SPOFs reveal fragility in the architecture |
| `ad-infinitum` | Flywheel designs need architectural grounding — loops must fit the structural model |
| `eccentric-billionaire` | Dares need architectural feasibility — bold moves require structural support |
| `goal-digger` | Priority ranking informs which parts of the architecture to formalize first |
| `amicus` | Stakeholder impacts inform governance design and decision-rights allocation |
| `elliot` | Adversarial findings reveal where the architecture can be exploited or gamed |
| `the-architect` | Software architecture feeds system architecture; they are complementary layers |
| `panopticon` | Architecture health signals feed situational awareness; panopticon routes architecture-relevant intelligence |
| `bottle-rocket` | Architecture hypotheses may need experiment validation before commitment |
| `the-thinker` | Durable capture of architecture patterns, rule designs, and constraint learnings |
| `polymath` | Model selection per pipeline stage |

---

## Output Contract

| Artifact | Path | Stage |
|----------|------|-------|
| Structural Map | `.state/matrix-architect/structural-map.json` | Decompose |
| Load-Bearing Analysis | `.state/matrix-architect/load-bearing-analysis.json` | Identify |
| Rule Audit | `.state/matrix-architect/rule-audit.json` | Rule |
| Rule Design | `.state/matrix-architect/rule-design.json` | Rule |
| Incentive Audit | `.state/matrix-architect/incentive-audit.json` | Align |
| Alignment Recommendations | `.state/matrix-architect/alignment-recommendations.json` | Align |
| Constraint Grid | `.state/matrix-architect/constraint-grid.json` | Constrain |
| Constraint Recommendations | `.state/matrix-architect/constraint-recommendations.json` | Constrain |
| Failure Map | `.state/matrix-architect/failure-map.json` | Harden |
| Hardening Plan | `.state/matrix-architect/hardening-plan.json` | Harden |
| Governance Layer | `.state/matrix-architect/governance-layer.json` | Formalize |
| Formalization Plan | `.state/matrix-architect/formalization-plan.json` | Formalize |
| Revision Schedule | `.state/matrix-architect/revision-schedule.json` | Formalize |
| Architecture Summary | `.state/matrix-architect/architecture-summary.md` | Formalize |
| Integrity Score | `.state/matrix-architect/integrity-score.json` | Audit |
| Architecture Debt | `.state/matrix-architect/architecture-debt.json` | Ongoing |
| Session Record | `.state/matrix-architect/session.packet.json` | All |
| Thinker Intake | `.state/matrix-architect/thinker-intake.packet.json` | All |

---

## Guardrails

1. **Formalize only what deserves formalization** — premature architecture suffocates emergence.
2. **Preserve flexibility where learning is still happening** — fluid areas should remain fluid until patterns stabilize.
3. **Optimize for system function, not mere neatness** — architecture serves the system, not the other way around.
4. **Keep humans, incentives, and real behavior in view** — a rule nobody follows is not architecture, it's decoration.
5. **Do not mistake a diagram for a working system** — structural maps are inputs to design, not substitutes for it.
6. **Design for imperfect participants** — systems that require perfect behavior are not systems, they are wishes.
7. **Always assess enforcement** — a rule without enforcement mechanism is a suggestion.
8. **Never impose architecture without decomposition** — understand before structuring.
9. **Always coordinate with polymath for model selection** — do not select mental models independently.
10. **Always emit thinker-intake with architecture patterns** — durable capture of structural knowledge.

---

## Escalation Paths

| Condition | Action |
|-----------|--------|
| Architecture debt > 30 | Emergency review — system operating on unformalized critical structures |
| Incentive misalignment severity = existential | Immediate alert to user — structure actively undermines core goals |
| Load-bearing element with bus factor = 1 | Flag for immediate redundancy design |
| Rule conflict on hard constraints | Escalate to `asimov` for constraint precedence resolution |
| Equilibrium tendency = strong disorder | Escalate to user for structural intervention decision |
| Formalization readiness = urgent on 5+ items | Trigger batch formalization session |
| Governance gaps on existential decisions | Escalate to `amicus` for stakeholder input on decision rights |

---

## Build Order

| Step | Files | Count |
|------|-------|-------|
| 1. Directory structure | `plugins/matrix-architect/{skills/matrix-architect/references, commands, agents, hooks}` | — |
| 2. SKILL.md | `skills/matrix-architect/SKILL.md` | 1 |
| 3. References | `skills/matrix-architect/references/*.md` | 8 |
| 4. Commands | `commands/*.md` | 9 |
| 5. Agents | `agents/*.md` | 4 |
| 6. Hooks manifest | `hooks/manifest.md` | 1 |
| 7. Hook handlers | `hooks/*.ts` | 14 |
| 8. Config + README | `package.json`, `tsconfig.json`, `marketplace.json`, `README.md` | 4 |
| 9. Bus-lint + commit | Validation and git commit | — |

**Total: ~41 files**
