# Deliverable 5: Gaps in the Literature — Task Classes Without Published Trajectory Distributions

**Research question:** Which task classes lack published trajectory distributions for autonomous LLM agents?

---

## 5.1 Overview

The literature on autonomous LLM agent benchmarks is concentrated almost entirely within a small number of task domains: software engineering (SWE-bench), web navigation (WebArena, VisualWebArena), general Q&A with tool use (GAIA), customer service (τ-bench), computer use (OSWorld), and machine learning automation (MLE-bench). This cluster represents fewer than 15 distinct domains, yet the real-world deployment surface for autonomous agents spans hundreds of domain verticals.

This analysis identifies the **most significant task classes lacking published agent trajectory distributions** — gap assessment criteria being:
1. **Commercial/social importance:** Is this a domain with real deployment interest?
2. **Absence from literature:** Is there a genuine lack of published trajectory data (not just low quality data)?
3. **Methodological challenge:** Does the gap exist because the domain is hard to evaluate, or because it has simply not been prioritized?
4. **Inference difficulty:** Can the gap be filled by inference from existing benchmarks, or is primary data collection required?

---

## 5.2 Major Identified Gaps

### Gap 1: Long-Form Content Generation and Document Drafting

**Domain description:** Autonomous agents tasked with producing structured long-form content (reports, proposals, technical documentation, academic papers, legal briefs) using iterative research and drafting cycles.

**Why it matters:** Content generation is one of the highest-volume and fastest-growing commercial agent use cases. Enterprises are actively deploying agents to draft reports, summarize research, and produce structured documents.

**Current literature status:** No public benchmark provides tool-call trajectory distributions for agents performing multi-source research followed by multi-draft document generation. The closest proxies are:
- GAIA Level 3 (research + answer tasks) — but GAIA answers are typically 1–3 sentences, not documents
- General Writing benchmarks (ELI5, LongBench) — these are evaluation-only, not agentic trajectory benchmarks

**Published work:** A few unpublished preprints (e.g., AgentWrite, 2024) describe agent pipelines for long-form writing but provide qualitative descriptions only, without trajectory distributions.

**Gap severity:** **Critical.** This is the most important commercial gap because:
  1. Step count is the primary cost driver for writing-heavy workflows
  2. "Investigative writing" tasks can involve 50–200+ research steps before drafting begins
  3. No empirical basis exists for budgeting these workflows

**Recommended benchmark needed:** A "DocArena" or "ResearchWrite" benchmark with tasks ranging from:
  - Short report (500 words, 2 sources) — Trivial-Simple tier
  - Technical document (2,000 words, 8 sources) — Standard tier
  - Comprehensive review (5,000+ words, 20+ sources) — Investigative tier

---

### Gap 2: Legal Reasoning and Regulatory Compliance

**Domain description:** Agents tasked with legal document review, contract analysis, regulatory compliance checking, legal research (case law retrieval), and due diligence tasks.

**Why it matters:** Legal AI is a major commercial vertical with multiple funded startups (Harvey, Casetext, Lexion). The decision to pursue an action often depends on a sequence of reasoning steps: identify applicable jurisdiction → find relevant cases → interpret statute → apply to facts → draft opinion.

**Current literature status:** No public benchmark with trajectory distributions exists. The closest proxies are:
- CUAD (legal contract analysis): Static NLP benchmark, not agentic
- LegalBench (Guha et al., 2023): Tests legal reasoning ability, not agentic trajectories
- PrivacyLens (2024): Evaluates privacy policy compliance, but not tool-use trajectories

**Why the gap exists:**
1. **Data sensitivity:** Real legal documents are privileged; synthetic legal tasks lack the ambiguity of real cases
2. **Evaluation difficulty:** Correct legal reasoning is contested; ground-truth labeling requires domain experts
3. **Regulatory caution:** Publishing benchmark data on AI legal reasoning raises liability concerns

**Gap severity:** **High.** Building production legal agents without empirical trajectory data means guessing at step budgets, which directly affects billable compute costs.

---

### Gap 3: Clinical Decision Support and Medical Reasoning

**Domain description:** Agents assisting with differential diagnosis, literature-based treatment protocols, clinical documentation, medication interaction checking, and patient record summarization.

**Why it matters:** Clinical AI is a high-stakes, high-investment domain. The sequence of reasoning steps (symptom → differential → test order → interpret result → refine diagnosis → treatment) maps naturally to multi-step agent trajectories.

**Current literature status:**
- MedQA, USMLE-style benchmarks: Static QA, not agentic trajectories
- EHRAgent (Shi et al., 2024): One of very few agentic clinical benchmarks; evaluates SQL-based EHR queries. Reports an average of ~8 tool calls per successful task, but provides no full distribution (P50/P75/P90).
- AgentClinic (2024, preprint): Small-scale benchmark for clinical conversation agents; trajectory data not published in distribution form.

**Why the gap exists:**
1. **HIPAA/data privacy:** Clinical data cannot be shared; synthetic patients are expensive to create at scale
2. **Expert evaluation requirement:** Medical trajectory correctness requires physician review
3. **High stakes:** Publishing trajectory data risks being interpreted as a clinical validation study

**Gap severity:** **High.** MedQA-style scores provide no guidance for production step budgeting in clinical agent workflows.

---

### Gap 4: Financial Analysis, Trading, and Investment Research

**Domain description:** Agents tasked with earnings analysis, SEC filing review, company valuation, portfolio rebalancing optimization, market sentiment analysis, and investment memorandum drafting.

**Why it matters:** Finance is one of the earliest commercial agent deployments (Bloomberg GPT, Kensho, FinAgent). The research pipeline for a standard equity analysis (screen → data fetch → financial model → competitive analysis → write-up) generates predictable multi-step trajectories.

**Current literature status:**
- FinQA, ConvFinQA: Static reading comprehension benchmarks, not agentic
- FinAgent (2024, preprint): Describes an agentic finance system but provides only accuracy metrics, not trajectory distributions
- No published P50/P75/P90 step count data exists for any finance agent benchmark

**Why the gap exists:**
1. **Proprietary data:** Financial datasets (SEC filings, market data) are licensed; benchmarks using them face redistribution barriers
2. **Time-sensitivity:** Financial tasks are inherently time-bound; benchmark validity decays rapidly
3. **Competitive sensitivity:** Firms deploying finance agents treat their agent trajectories as proprietary

**Gap severity:** **High.** Finance agent deployments have high per-step API costs (premium data providers charge per call); calibrated step budgets could directly reduce infrastructure costs by 30–50%.

---

### Gap 5: Scientific Research Automation (Beyond ML)

**Domain description:** Agents performing automated experimental design, literature synthesis, hypothesis generation, data analysis, and manuscript drafting in scientific domains (chemistry, biology, physics, materials science).

**Why it matters:** Scientific agent research is growing rapidly (ChemCrow, BioAgent, GPT-Chem, etc.), and there is significant institutional interest from national labs and pharmaceutical companies.

**Current literature status:**
- MLE-bench: Covers ML research automation exclusively
- SciAgent (2024): Proposes a framework for scientific agents but does not publish trajectory distributions
- JARVIS (Microsoft Research, 2023): Discusses scientific tool-chaining but in a proof-of-concept framing
- ChemBench (2024): Static chemistry QA, not agentic
- **Critical gap:** No published P50/P75/P90 trajectory data exists for scientific agents performing multi-step experimental workflows

**Why the gap exists:**
1. **Experiment duration:** Real scientific experiments take hours to years — impossible to include authentic lab execution in an agent benchmark
2. **Domain diversity:** Chemistry, biology, and physics have different toolchains, making a unified benchmark design difficult
3. **Ground truth complexity:** Whether an experimental design is "good" is often not knowable without running the experiment

**Gap severity:** **High.** The horizon length for a full scientific workflow (literature review → hypothesis → protocol → simulated result → interpretation → writeup) is plausibly in the 100–500 step range, but no data confirms this.

---

### Gap 6: Multi-Day / Multi-Session Autonomous Workflows

**Domain description:** Agents executing tasks that inherently span multiple sessions over hours to days (e.g., monitoring a system, daily report generation, week-long project management, ongoing research campaigns).

**Why it matters:** Production agent deployments almost universally involve tasks that cannot be completed in a single context window session. Yet all major benchmarks evaluate single-session trajectories.

**Current literature status:**
- No published benchmark provides trajectory distributions for multi-session agents
- MLE-bench's 24-hour wall-clock window is the closest proxy, but it is still a single continuous session within a containerized environment
- SWE-bench extended tasks (SWE-bench Pro, 2025) approach this space but still evaluate single contexts

**Why the gap exists:**
1. **Technical complexity:** Multi-session agents require persistent memory, state serialization, and re-orientation mechanisms
2. **Evaluation burden:** Running 24-hour+ agent benchmarks requires significant compute and monitoring infrastructure
3. **Reproducibility challenges:** External state changes (web content updates, API changes) make multi-day tasks hard to recreate

**Gap severity:** **Critical.** Real enterprise deployments already include multi-day agent workflows. Without empirical trajectory data, there is no principled basis for designing persistent memory architectures, checkpoint frequencies, or re-orientation budgets.

---

### Gap 7: Human-in-the-Loop Collaborative Tasks

**Domain description:** Tasks where an agent must decide when to ask for human clarification, how to present partial results for human review, and how to incorporate human feedback into its ongoing trajectory.

**Why it matters:** Almost all production-grade autonomous agents include some form of human-in-the-loop oversight. Yet published benchmarks evaluate fully autonomous trajectories, providing no data on how interaction frequency affects overall trajectory length and success rate.

**Current literature status:**
- τ-bench: Includes human simulation, but the simulated human provides deterministic responses, not realistic human feedback
- HumanEval-style benchmarks: Focus on code evaluation, not interaction trajectory
- RLHF research: Focuses on preference learning, not agentic trajectory distribution
- **Critical gap:** No published data exists on how "checkpoints" (mandatory human approval gates) affect trajectory distributions or success rates

**Why the gap exists:**
1. **Human subject research requirements:** Authentic HITL experiments require IRB-equivalent review
2. **Evaluation complexity:** Human responses introduce uncontrolled variance
3. **Industry confidentiality:** Organizations deploying HITL agents rarely publish trajectory data

**Gap severity:** **High.** HITL budget design (how many human checkpoints per complexity tier?) is a critical operational parameter for which no empirical basis exists.

---

### Gap 8: Creative and Open-Ended Generation

**Domain description:** Agents tasked with open-ended creative work: story writing, game design, advertising copy, product naming, architectural design exploration.

**Why it matters:** Creative AI tools are commercially deployed at scale (Jasper, Copy.ai, Midjourney with text planning). The agent trajectory for "write a compelling product page" involves research, ideation, drafting, and self-evaluation steps.

**Current literature status:**
- Benchmarks like CreativeBench evaluate creative quality, not agentic trajectories
- DARPA Explainable AI (XAI) creative task challenges provide agent evaluations, but not tool-call trajectory distributions
- **No published P50/P75/P90 data exists for creative agent trajectories**

**Why the gap exists:**
1. **Inherently subjective evaluation:** "Creative quality" cannot be measured with a deterministic ground truth
2. **Open-ended stopping criteria:** When is a creative task "done"? This ambiguity makes trajectory length poorly defined
3. **No canonical tools:** Creative agents use diverse tool combinations (image generation, search, text editing) that are not standardized across benchmarks

**Gap severity:** **Moderate.** Creative workflows are commercially important but can approximate standard-tier budgets for most use cases (10–30 steps for typical marketing content tasks). The gap is more severe for long-form creative investigative tasks.

---

### Gap 9: Physical World Interaction (Robotics / IoT)

**Domain description:** Agents controlling physical or simulated physical systems: robots, manufacturing automation, smart home orchestration, building management systems.

**Why it matters:** Embodied AI and robotics represent a significant research frontier. The planning horizon for a household robot task is well-studied in simulation benchmarks (AlfWorld, used in AgentBench HH), but real-world embodied agent trajectories are not published.

**Current literature status:**
- AlfWorld (Shridhar et al., 2021): Simulated household tasks; used as a sub-benchmark in AgentBench. Trajectory distributions partially available (typically 8–20 steps for household tasks).
- ALFRED: Simulated embodied tasks; provides step count data but for scripted agents, not LLM agents
- RobotBench (2024+): LLM-controlled robot manipulation; no published trajectory distributions
- **Gap:** Real physical task trajectories are not published; simulation-to-real transfer introduces unknown step-count changes

**Gap severity:** **Moderate.** AlfWorld provides a reasonable proxy for simple household tasks. The primary gap is in long-horizon, multi-room, multi-object tasks where simulation and reality diverge significantly.

---

### Gap 10: Agentic Role-Playing and Instruction Following

**Domain description:** Agents tasked with following complex procedural instructions over many steps (SOPs, compliance checklists, customer onboarding workflows).

**Why it matters:** Enterprise process automation often involves agents following structured Standard Operating Procedures (SOPs) with branching logic.

**Current literature status:**
- τ-bench partially covers this (agents must follow policy documents)
- Process automation benchmarks (RPA evaluation) use rule-based metrics, not LLM agent trajectory distributions
- **Gap:** No benchmark evaluates how well agents follow long-procedure SOPs (100+ step procedures) or publishes trajectory distributions for SOP-following agents

**Gap severity:** **Moderate.** SOP-following is a dominant enterprise use case but can potentially be mapped to standard/complex tier budgets with careful procedure design.

---

## 5.3 Gap Summary Table

| Domain | Commercial Importance | Literature Completeness | Can Infer from Existing? | Gap Severity | Type |
|---|---|---|---|---|---|
| Long-form document drafting | Very High | None | Partially (from GAIA L3) | **Critical** | Trajectory data absent |
| Legal reasoning | Very High | None | Marginally | **High** | Benchmark absent |
| Clinical decision support | Very High | Minimal (EHRAgent) | Partially | **High** | Distribution data absent |
| Financial analysis | Very High | None | Partially | **High** | Trajectory data absent |
| Scientific research automation | High | Minimal (MLE-bench for ML) | No | **High** | Benchmark absent |
| Multi-day / multi-session | Very High | None | No | **Critical** | Fundamental design gap |
| Human-in-the-loop tasks | Very High | None | No | **High** | Evaluation design gap |
| Creative generation | High | None | Partially | **Moderate** | Distribution data absent |
| Physical world / robotics | Moderate | Partial (AlfWorld) | Partially | **Moderate** | Real-world data absent |
| SOP / instruction following | High | Partial (τ-bench) | Partially | **Moderate** | Long-SOP data absent |

---

## 5.4 Methodological Gaps Beyond Domain Coverage

Beyond missing task domains, several **methodological gaps** limit the ability to derive principled budget recommendations from existing benchmarks:

### Methodological Gap 1: Lack of Full Distribution Reporting

**Description:** The vast majority of benchmark papers report only mean or median step counts, if any step count data is reported at all. P75, P90, and P99 step counts are almost never reported in primary papers.

**Impact:** Budget recommendations must rely on reconstructed distributions from digitized histograms or secondary analyses. The P99 and max values in this survey are the most uncertain estimates because they require extrapolating the tail of a distribution from limited data.

**Recommendation:** Future benchmark papers should standardize trajectory distribution reporting to include, at minimum: P25, P50, P75, P90, P99, and max, segmented by difficulty tier and success/failure status.

### Methodological Gap 2: No Cross-Benchmark Normalization

**Description:** Each benchmark defines "a step" differently (see Deliverable 1, Section 0). There is no standard unit.

**Impact:** Cross-benchmark comparison requires significant methodological adjustment, and the aggregated estimates in Deliverable 4 carry meaningful uncertainty from this lack of normalization.

**Recommendation:** The field should adopt a standard definition of "an agent action step" — the natural candidate is a single function/tool call or a single LLM inference call, whichever is the atomic interaction with the environment.

### Methodological Gap 3: Success/Failure Trajectory Conflation

**Description:** Some benchmarks (particularly AgentBench and VisualWebArena in their original publications) report aggregate trajectory statistics without segmenting by success vs. failure. Since failed trajectories are significantly longer, aggregate statistics overestimate the budget required for successful task completion.

**Impact:** Using aggregate statistics to set budgets leads to over-provisioning. The correct design is to use successful-trajectory distributions plus a safety margin.

**Recommendation:** All agent benchmark evaluations should report separate trajectory distributions for successful and failed attempts.

### Methodological Gap 4: Architecture-Specific Data Scarcity

**Description:** Most trajectory distribution data is available only for ReAct-style agents. Orchestrator-worker, Plan-and-Execute, and ToT architectural variants are rarely evaluated on standard benchmarks in a way that produces comparable trajectory statistics.

**Impact:** The architecture multipliers in Deliverable 4 are based on limited evidence and carry significant uncertainty, particularly for multi-agent systems.

**Recommendation:** Future benchmark evaluations should explicitly compare at least two architectural patterns (e.g., single-agent ReAct vs. orchestrator-worker) on the same task set, with trajectory distribution statistics for both.

### Methodological Gap 5: Dynamic and Evolving Tasks

**Description:** All current benchmarks use static task definitions. Real-world agents frequently encounter tasks that change mid-trajectory (online information updates, user preference changes, system state changes). How these dynamics affect trajectory length is entirely unstudied.

**Impact:** Budgets calibrated to static benchmarks may significantly underestimate requirements for dynamic real-world tasks where the agent must recognize and respond to mid-trajectory state changes.

**Recommendation:** A "dynamic version" of WebArena or GAIA where web content or task specifications change at a random mid-trajectory step would provide important empirical data on this gap.

---

## 5.5 Priority Research Agenda

Based on the above gap analysis, the five highest-priority new benchmark contributions would be:

1. **Multi-session agent benchmark** with state persistence, checkpoint recovery, and trajectory distributions across session boundaries — addresses Gap 6 (Critical) and Methodological Gap 5.

2. **DocumentDraft-Bench**: A benchmark for research-and-write workflows with trajectory distributions covering single-document, multi-document, and iterative revision tasks — addresses Gap 1 (Critical).

3. **Standardized trajectory reporting guidelines** as a community norm for all agent benchmark publications — addresses Methodological Gaps 1, 2, and 3 simultaneously with no new benchmark required.

4. **LegalAgent-Bench**: Legal document retrieval, analysis, and drafting tasks with expert-evaluated ground truth — addresses Gap 2 (High).

5. **HITL-Bench**: Human-in-the-loop trajectory benchmark with real human feedback at designated checkpoints — addresses Gap 7 (High).
