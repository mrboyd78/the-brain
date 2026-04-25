# Deep Research Prompts for LLM-Only SaaS and Third-Party App Research

## Purpose
These prompts are designed for deep research conducted entirely through LLM-assisted analysis of publicly available information, without human interviews, customer surveys, or direct field research. They are built to help identify software opportunities that remain commercially viable even as AI-assisted coding becomes more accessible.

Each prompt is structured to push the model beyond shallow idea generation and into evidence-based strategic analysis. The prompts should be run with instructions that require:

- reliance on current, reputable, publicly available sources
- explicit citation of claims
- separation of facts from inference
- identification of disagreement, uncertainty, and blind spots
- commercial interpretation rather than generic summary

---

## Master Operating Instruction
Use this instruction before any of the prompts below.

```text
Conduct this research using only publicly available information. Do not use interviews, surveys, invented examples, or unsupported speculation. Use current sources where recency matters. Distinguish clearly between:
1. verified facts,
2. reasoned inference,
3. uncertain or contested conclusions.

For every major claim, cite sources. Prioritize primary sources, official documentation, platform policies, earnings materials, credible industry research, security reports, reputable review ecosystems, and high-quality operator commentary when directly relevant.

Your task is not merely to summarize. Your task is to identify commercially meaningful patterns, market tensions, buyer incentives, implementation burdens, and strategic opportunities.

For each section of the answer, include:
- what is known,
- what appears to be changing,
- what market participants may be underestimating,
- what this implies for a third-party app or SaaS builder.

Where evidence is weak, say so explicitly.
```

---

## Prompt 1 — What Software Will People Still Pay For in the Age of AI Coding?

```text
Conduct deep research on the question: “What kinds of SaaS products and third-party apps will customers still be willing to pay for even as AI-assisted coding and agentic software creation become more accessible to non-coders?”

Research this across SMB, mid-market, and enterprise contexts.

Your analysis must include:
1. how AI coding changes the supply of software,
2. what it does not eliminate from buyer decision-making,
3. what still creates willingness to pay,
4. which software categories are being commoditized,
5. which categories are becoming more valuable,
6. which moats still matter,
7. how trust, implementation burden, security, governance, maintenance, domain expertise, and integration depth affect software purchasing.

Then produce:
- a taxonomy of software categories by durability of willingness to pay,
- a list of fragile product types likely to be commoditized,
- a list of resilient product types likely to retain pricing power,
- a buyer psychology section explaining why customers buy instead of build,
- a final strategic framework for choosing ideas worth building.

Cite all substantive claims. Use current sources.
```

---

## Prompt 2 — Identify the Real Moats That Survive AI-Assisted Software Creation

```text
Conduct deep research on the durable moats available to SaaS founders and third-party app developers in a world where software generation is becoming easier.

Do not give generic startup advice. Ground the analysis in evidence.

Evaluate the comparative strength of these possible moats:
- proprietary data
- distribution within a platform ecosystem
- workflow ownership
- trust and compliance
- integration depth
- implementation complexity
- service layer and support
- switching costs
- domain-specific judgment
- community and network effects
- embedded operational knowledge
- auditability and governance

For each moat:
- define it precisely,
- explain how AI may weaken or strengthen it,
- assess how realistic it is for a small independent developer,
- describe how it can be built intentionally,
- identify warning signs of a fake moat.

Conclude with a ranked model of which moats are strongest for a solo or small-team app builder in 2026.

Use evidence and citations throughout.
```

---

## Prompt 3 — Which Third-Party App Ecosystems Still Offer Strong Opportunities?

```text
Conduct deep research on which third-party app ecosystems still present strong commercial opportunity for independent developers and small teams.

Compare major ecosystems such as:
- Shopify
- HubSpot
- Webflow
- Notion
- Airtable
- Slack
- Microsoft 365 / Teams
- Google Workspace
- Figma
- Salesforce
- Atlassian
- Stripe
- Discord
- WordPress
- Canva
- emerging AI-native platforms if relevant

For each ecosystem, evaluate:
- size and maturity of ecosystem,
- app store discoverability,
- technical barriers to entry,
- policy or approval constraints,
- monetization patterns,
- prevalence of low-quality commodity apps,
- white space opportunity,
- types of customers served,
- likelihood of platform feature encroachment,
- integration and workflow complexity,
- whether an independent builder can still meaningfully win.

Then create:
- a comparative ranking,
- a “best for solo builders” shortlist,
- a “best for higher-ticket operational apps” shortlist,
- a “most dangerous due to platform copy risk” shortlist,
- a recommendation section identifying the most strategically attractive ecosystems and why.

Use current publicly available sources and cite all claims.
```

---

## Prompt 4 — What Problems Are Customers Too Lazy, Busy, or Risk-Averse to Solve Themselves?

```text
Conduct deep research on software problems that customers could theoretically build or patch together themselves using AI and no-code tools, but in practice still prefer to pay someone else to solve.

Focus on the gap between theoretical buildability and real-world purchase behavior.

Investigate evidence for why customers still buy instead of build, including:
- time constraints,
- implementation fatigue,
- integration burden,
- hidden edge cases,
- maintenance burden,
- security concerns,
- governance concerns,
- fear of breaking production workflows,
- lack of reliable product judgment,
- reluctance to own a brittle internal tool.

Identify recurring classes of problems where this gap is strongest.

Then produce:
- a taxonomy of “too annoying to build” software problems,
- examples of workflow categories where DIY consistently fails or stalls,
- a framework for spotting high-payment-likelihood problems,
- implications for product design, pricing, onboarding, and support.

Use evidence rather than intuition. Cite sources.
```

---

## Prompt 5 — Detect White Space in Crowded SaaS Categories

```text
Conduct deep research on how to identify commercially meaningful white space inside crowded software categories.

The goal is not to find empty categories. The goal is to find underserved jobs, underbuilt workflow layers, neglected customer segments, weak implementation experiences, or governance gaps inside categories that already appear saturated.

Your analysis must include:
- why crowded markets can still produce winners,
- how to distinguish superficial saturation from strategic exhaustion,
- how to detect under-served segments through public evidence,
- how to read reviews, complaints, changelogs, documentation gaps, and support pain points as signals,
- how platform constraints create openings,
- how pricing mismatches reveal white space,
- how to identify opportunities in services-heavy or spreadsheet-heavy workflows.

Then create a repeatable white-space detection method that a founder can run entirely through public research.

End with a scoring model for evaluating whether a perceived gap is commercially real or merely interesting.

Use citations throughout.
```

---

## Prompt 6 — Build-vs-Buy Dynamics in 2026

```text
Conduct deep research on modern build-vs-buy dynamics for software buyers in 2026, especially in contexts where AI coding tools and agentic systems have lowered perceived build costs.

Evaluate:
- how buyers currently decide whether to build internally, buy off-the-shelf, or combine both,
- how the rise of AI-assisted development changes internal software economics,
- what hidden costs still make internal builds unattractive,
- how security, support, accountability, and maintenance shape buying decisions,
- how this differs across SMB, mid-market, and enterprise buyers.

Then produce:
- a build-vs-buy decision framework,
- the strongest reasons buyers still purchase external software,
- the strongest reasons they choose internal builds,
- the classes of products most vulnerable to in-house replacement,
- the classes of products least vulnerable to in-house replacement.

Conclude with direct implications for a third-party app founder choosing what to build.

Use current and cited sources.
```

---

## Prompt 7 — How to Find High-Value Workflow Pain From Public Evidence Alone

```text
Conduct deep research on how to identify high-value workflow pain using only publicly available information.

Do not use interviews or surveys. Build a method using sources such as:
- product reviews,
- documentation,
- community forums,
- implementation guides,
- public support threads,
- changelogs,
- release notes,
- marketplace listings,
- GitHub issues if relevant,
- analyst reports,
- earnings materials,
- security and compliance reports,
- industry blogs from operators.

Your task is to create a research method for inferring:
- where users lose time,
- where workflows break,
- where manual work persists,
- where compliance risk accumulates,
- where teams rely on spreadsheets or ad hoc procedures,
- where the category’s current products are under-serving users.

Then provide:
- a repeatable process,
- a source hierarchy by evidentiary value,
- warning signs of misleading signals,
- a pattern library of pain indicators,
- a final methodology for turning public evidence into product hypotheses.

Cite all material claims.
```

---

## Prompt 8 — The Best Types of SaaS to Build as a Small Team in the AI Era

```text
Conduct deep research on what types of SaaS products are best suited to solo founders or small teams in the AI era.

The answer should not be generic. Assess what is strategically favorable for a small builder given current market conditions.

Evaluate categories according to:
- technical complexity,
- support burden,
- implementation burden,
- security exposure,
- required sales motion,
- ease of distribution,
- ability to prove ROI quickly,
- defensibility,
- susceptibility to platform copy risk,
- ability to benefit from AI-assisted development without being commoditized by it.

Identify categories or business models that are especially favorable for small teams, such as:
- narrow but painful workflow tools,
- admin and governance tools,
- integration products,
- implementation accelerators,
- embedded analytics tied to action,
- vertical operational tools,
- software-plus-service hybrids.

Then produce a ranked list of the most strategically attractive SaaS/app categories for small builders, with reasoning and citations.
```

---

## Prompt 9 — What Buyers Regret About SaaS Purchases and How That Creates Opportunity

```text
Conduct deep research on why buyers regret SaaS purchases and how those regret patterns create openings for better products.

Your analysis should examine public evidence related to:
- failed implementation,
- poor onboarding,
- weak customer support,
- overpromised AI features,
- pricing confusion,
- poor integration quality,
- security concerns,
- missing governance features,
- unreliable automation,
- feature bloat,
- underpowered reporting,
- inability to fit actual workflows.

Use this evidence to determine:
- which forms of disappointment are most common,
- which are most commercially exploitable,
- which can be solved by a smaller, sharper product,
- how founders can design against regret from the beginning.

End with a product design doctrine for building software that customers are less likely to regret buying.

Use current cited sources.
```

---

## Prompt 10 — Security, Trust, and Governance as Commercial Moats

```text
Conduct deep research on how security, trust, governance, auditability, and operational accountability function as commercial differentiators for SaaS and third-party apps.

Do not treat these as technical afterthoughts. Treat them as revenue-relevant product features.

Research:
- whether customers are placing increasing weight on security and governance,
- how SaaS sprawl and AI adoption affect buyer anxiety,
- what kinds of trust failures create purchase friction,
- how smaller vendors can still win trust,
- which governance features are especially valuable in operational software,
- how audit trails, permissions, logs, approvals, and policy controls affect willingness to pay.

Then conclude with:
- a framework for when trust features are central rather than secondary,
- product categories where governance is a core moat,
- recommendations for independent developers on where trust can be turned into a premium advantage.

Cite all claims.
```

---

## Prompt 11 — Where AI Features Add Real Value Versus Mere Commodity Noise

```text
Conduct deep research on the difference between AI features that create real commercial value and AI features that amount to commodity noise.

Your analysis must identify:
- which AI product patterns are already becoming table stakes,
- which AI features are easily copied,
- which AI applications become more valuable when embedded into workflows,
- where AI should act as assistant versus operator,
- what evidence exists that buyers are tiring of superficial AI claims,
- where auditable or governed AI creates stronger value.

Then create a framework for evaluating whether an AI feature:
- improves outcomes,
- reduces cost,
- reduces risk,
- increases speed,
- strengthens lock-in,
- or merely decorates the product.

End with recommendations for how a third-party app developer should use AI in a way that supports monetization rather than cheapening the product.

Use current sources and citations.
```

---

## Prompt 12 — A Research Framework for Choosing One SaaS Idea Worth Building

```text
Conduct deep research to create a rigorous framework for selecting one SaaS or third-party app idea that is worth building in the current market.

The framework must be usable without interviews or surveys. It must rely on publicly available evidence.

The output should include:
1. a scoring model for market attractiveness,
2. a scoring model for pain severity,
3. a scoring model for monetization likelihood,
4. a scoring model for defensibility,
5. a scoring model for implementation feasibility for a small team,
6. a scoring model for distribution viability,
7. a scoring model for platform dependency risk,
8. a scoring model for AI commoditization risk.

Then explain how to combine these into a final weighted decision model.

Finally, provide a worked example that shows how a founder would evaluate three candidate ideas using the framework.

Every scoring dimension must be justified with evidence and citations where appropriate.
```

---

## Prompt 13 — Public-Evidence Competitive Intelligence for SaaS Founders

```text
Conduct deep research on how a SaaS founder can perform competitive intelligence using only public information.

Build a method that analyzes competitors through:
- pricing pages,
- product pages,
- reviews,
- release notes,
- public roadmaps if available,
- changelogs,
- support docs,
- implementation docs,
- legal and security pages,
- marketplace listings,
- integration catalogs,
- customer case studies,
- public complaints and comparisons.

The output must explain how to infer:
- target customer segment,
- product maturity,
- support burden,
- likely retention strengths,
- implementation weakness,
- positioning gaps,
- pricing strategy,
- where competitors are overbuilt or underbuilt.

Then create a reusable competitor analysis template designed for founders deciding how to differentiate.

Use evidence, not guesswork.
```

---

## Prompt 14 — Productized Service Versus Pure SaaS: Which Wins Now?

```text
Conduct deep research on whether independent software builders are better served today by pure SaaS, productized services, or hybrid software-plus-service models.

Assess:
- current buyer willingness to pay for implementation help,
- whether support and onboarding now matter more in a crowded market,
- where software alone is too weak a value proposition,
- where hybrid delivery creates stronger retention or pricing power,
- where services become a trap rather than an advantage.

Compare pure SaaS, productized services, and hybrids on:
- speed to revenue,
- scalability,
- defensibility,
- customer trust,
- implementation success,
- founder burden,
- support intensity,
- long-term enterprise readiness.

Conclude with a decision framework for choosing the right model for a small independent app builder.

Use citations and current evidence.
```

---

## Prompt 15 — Find SaaS Opportunities Hidden Inside Manual Operations

```text
Conduct deep research on how to identify SaaS and third-party app opportunities hidden inside workflows that are still managed manually, through spreadsheets, ad hoc procedures, checklists, email coordination, or internal admin labor.

Use public evidence to identify where manual operational burden persists despite an apparently mature software market.

Investigate:
- symptoms of spreadsheet dependence,
- signs of brittle operational processes,
- repeated handoff failures,
- public complaints about multi-tool workflows,
- categories where current products stop at visibility rather than execution,
- categories where teams still rely heavily on human QA or exception handling.

Then produce:
- a framework for identifying manual-operation opportunities,
- indicators that the workflow is valuable enough to monetize,
- indicators that automation would be welcomed rather than resisted,
- guidance for when to build tooling versus full workflow ownership.

Cite claims throughout.
```

---

## Prompt 16 — A Red-Team Prompt to Stress-Test Any SaaS Idea

```text
I am considering building a SaaS or third-party app around this idea: [INSERT IDEA].

Conduct a deep research-based red-team critique using only publicly available information.

Do not encourage the idea by default. Try to break it.

Evaluate:
- whether the pain is real,
- whether the market is already saturated in a meaningful way,
- whether incumbents or the host platform can easily copy it,
- whether buyers actually pay for this category,
- whether implementation will be harder than it appears,
- whether the value proposition is too shallow,
- whether retention risk is high,
- whether the product is vulnerable to AI commoditization,
- whether this idea depends on false assumptions about customer behavior.

Then provide:
- the strongest arguments against building it,
- the strongest evidence in favor of building it,
- the assumptions that must be true for it to work,
- the key unknowns that public research cannot fully resolve,
- a final verdict: kill, reconsider, narrow, or pursue.

Use evidence and citations throughout.
```

---

## Recommended Usage Sequence

Run the prompts in this order if you want to move from strategy to idea selection:

1. Prompt 1
2. Prompt 2
3. Prompt 3
4. Prompt 4
5. Prompt 5
6. Prompt 6
7. Prompt 7
8. Prompt 8
9. Prompt 9
10. Prompt 10
11. Prompt 11
12. Prompt 12
13. Prompt 13
14. Prompt 14
15. Prompt 15
16. Prompt 16

---

## Optional Instruction for Higher Rigor

Attach this to any prompt when you want stricter output quality.

```text
Before finalizing the answer, perform an explicit internal adversarial review of your own conclusions. Identify at least five ways your reasoning may be weak, incomplete, biased, overly optimistic, or too dependent on limited public evidence. Then revise the answer accordingly.
```

---

## Optional Output Format

Use this output structure for consistency across prompts.

```text
1. Executive Thesis
2. What the Evidence Shows
3. What Appears to Be Changing
4. Where the Market Is Misreading the Situation
5. Strategic Implications for a SaaS / Third-Party App Builder
6. Risks, Counterarguments, and Uncertainty
7. Final Recommendations
8. Source List
```

