---
name: competitive-intel-deck
description: Builds a cited competitive brief + slide deck analyzing Microsoft Fabric competitors' supportability and support ecosystem. Uses Work IQ + web search + ppt-creator.
argument-hint: Research Fabric competitors (Snowflake, Databricks, Google BigQuery) focusing on support experience
---

## Goal

Produce a competitive intelligence brief and presentation deck under `docs/Lab4/work/` analyzing how Microsoft Fabric's direct competitors handle **supportability experience** and **support ecosystem**.

## Product Context

- **Our Product**: Microsoft Fabric — unified analytics platform combining data engineering, data science, real-time analytics, and business intelligence
- **Target Buyer**: Data platform teams, analytics leaders, enterprise architects evaluating unified data platforms

## Direct Competitors to Research

| Competitor | Focus Areas |
|------------|-------------|
| **Snowflake** | Support tiers, community forums, partner ecosystem, documentation quality |
| **Databricks** | Support SLAs, training/certification, partner network, troubleshooting resources |
| **Google BigQuery** | GCP support integration, community resources, enterprise support options |
| **AWS Redshift** | AWS support tiers, TAM model, knowledge base, partner solutions |

## Research Focus: Supportability & Support Ecosystem

For each competitor, gather:

### Support Tiers & SLAs
- Support plan options (free, standard, premium, enterprise)
- Response time SLAs by severity
- 24/7 availability, dedicated support contacts
- Pricing for support tiers

### Self-Service Resources
- Documentation quality and completeness
- Knowledge base / troubleshooting guides
- Community forums and activity levels
- Sample code repositories and tutorials

### Partner & Training Ecosystem
- Certified partner programs
- Training and certification paths
- Professional services availability
- System integrator partnerships

### Customer Experience Signals
- G2/Gartner Peer Insights support ratings
- Public feedback on support quality
- Case studies mentioning support experience

## Operating Rules

- **Do not fabricate facts.** Every concrete public claim needs a source URL.
- **Summarize web sources** — never dump full page content into context (STM discipline).
- **Separate internal vs public** — label Work IQ findings as "Internal Context", not public fact.
- Keep tone crisp, neutral, and stakeholder-friendly.
- Before running any skill, install its required packages (e.g. `npm install pptxgenjs` or `pip install python-pptx markitdown`).

## Subagent Orchestration

Execute the following subagents in order:

### Phase 1: Collectors (PARALLEL)

Run collection in parallel — one Collector subagent per competitor:

- **Collector-Snowflake, Collector-Databricks, Collector-BigQuery, Collector-Redshift**
- Each Collector uses the **web-search skill** to find:
  - Support tier options and pricing
  - SLA commitments (response times, availability)
  - Documentation and self-service resources
  - Partner/training ecosystem
  - Customer reviews of support experience (G2, Gartner Peer Insights)
- Each Collector writes a summary to `docs/Lab4/work/sources/<competitor-name>.md`
- Summary format must include:
  - Key claims (3-5 bullets)
  - Specific data (pricing, features)
  - Source URLs for each claim
  - Confidence level (High/Medium/Low)
- **Start all Collectors at the same time**
- Continue only after all Collectors have finished
- Create `docs/Lab4/work/sources/index.md` listing all competitor summaries

### Phase 2: Work IQ Context Gatherer

Use Work IQ CLI (`workiq ask`) to gather internal context:

```bash
workiq ask -q "What feedback have we received about Fabric support experience?"
workiq ask -q "How does our support model compare to Snowflake and Databricks?"
workiq ask -q "What are customers saying about Fabric documentation and self-service?"
workiq ask -q "What partner ecosystem gaps have been identified for Fabric?"
workiq ask -q "What support improvements are planned for Fabric?"
```

- Write all Work IQ findings to `docs/Lab4/work/internal-context.md`
- Label this as **Internal Context** — do not present as public fact

### Phase 3: Synthesizer

- Read all summaries from `docs/Lab4/work/sources/`
- Read internal context from `docs/Lab4/work/internal-context.md`
- Write `docs/Lab4/work/competitive-brief.md` containing:
  - Executive summary: key supportability gaps and opportunities
  - **Support Tier Comparison Table**: plans, SLAs, pricing across all competitors
  - **Self-Service Comparison**: documentation, community, knowledge base ratings
  - **Partner Ecosystem Comparison**: certifications, SI partnerships, training
  - SWOT analysis focused on support experience
  - Strategic recommendations for Fabric support improvements
  - All claims linked to source URLs

### Phase 4: Skeptic / Reviewer

- Review `docs/Lab4/work/competitive-brief.md`
- **Remove or soften** any claim without a source URL
- Add `[skepticNote: evidence thin]` where data is weak or speculative
- Flag outdated information
- Save updated brief in place

### Phase 5: Deck Builder

- Read the reviewed `docs/Lab4/work/competitive-brief.md`
- Use the **ppt-creator** skill to transform content into slides:
  - Apply Pyramid Principle structure
  - Use assertion-evidence headings (complete sentences, not topic labels)
  - Include comparison tables and key data points
- Use the **pptx** skill to generate the final `.pptx` file
- Output to `docs/Lab4/work/output/`
- Before running skills, install required packages:
  ```bash
  npm install pptxgenjs
  pip install python-pptx markitdown
  ```

## Output Files

| File | Purpose |
|------|---------|
| `docs/Lab4/work/sources/<competitor>.md` | Individual competitor summaries |
| `docs/Lab4/work/sources/index.md` | Index of all competitor summaries |
| `docs/Lab4/work/internal-context.md` | Work IQ internal findings |
| `docs/Lab4/work/competitive-brief.md` | Synthesized brief with comparison table |
| `docs/Lab4/work/output/*.pptx` | Final slide deck |

## Skills Used

- **web-search** — search public web and return summarized results
- **ppt-creator** — transform structured content into professional slides
- **pptx** — create and edit .pptx files directly
