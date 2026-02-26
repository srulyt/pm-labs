```skill
---
name: web-search
description: 'Search the public web for information and return summarized results. Use when asked to research competitors, find pricing information, look up product features, gather market intelligence, or search for any public information online. Triggers: web search, internet search, look up online, research competitors, find information about, google, bing, search the web.'
---

# Web Search Skill

Search the public web for a query and return concise, summarized results with source URLs. Designed to keep the agent's context window lean by returning summaries rather than full page content.

## When to Use This Skill

- User asks to "search the web", "look up", or "research" a topic
- User needs competitive intelligence (pricing, features, positioning)
- User wants to find public information about companies, products, or services
- User asks to "google" or "bing" something
- User needs current information not available in the codebase

## Prerequisites

- The `fetch_webpage` tool must be available
- A clear search query or topic to research

## Workflow

### Step 1: Construct Search URLs

Build search URLs using a search engine. Use Bing as the primary search engine:

```
https://www.bing.com/search?q=<URL-encoded-query>
```

For targeted searches, append site-specific modifiers:
- Company info: `<company name> pricing features`
- Product comparison: `<product A> vs <product B>`
- Official sources: `<query> site:microsoft.com`

### Step 2: Fetch and Extract Content

Use the `fetch_webpage` tool to retrieve search results and relevant pages:

1. **First pass**: Fetch the search results page to identify relevant URLs
2. **Second pass**: Fetch the top 2-3 most relevant result pages directly

Example tool call:
```
fetch_webpage(
  urls: ["https://www.bing.com/search?q=competitor+pricing+2026"],
  query: "pricing plans features cost"
)
```

### Step 3: Summarize Findings

**CRITICAL**: Return SUMMARIES, not full page content. This keeps the context window lean.

Each summary should include:

| Element | Description |
|---------|-------------|
| **Source URL** | The exact URL where information was found |
| **Key Claims** | 3-5 bullet points of main findings |
| **Specific Data** | Pricing numbers, feature lists, dates |
| **Confidence** | High/Medium/Low based on source credibility |

### Step 4: Format Output

Return results in this format:

```markdown
## Search Results: <query>

### Source 1: <Title>
- **URL**: <source-url>
- **Key Findings**:
  - <bullet point 1>
  - <bullet point 2>
  - <bullet point 3>
- **Confidence**: <High/Medium/Low>

### Source 2: <Title>
...
```

## Best Practices

### DO:
- Summarize aggressively — aim for 100-200 words per source
- Always include source URLs for every claim
- Prioritize official sources (company websites, press releases)
- Note when information may be outdated
- Mark pricing/features with the date found if available

### DON'T:
- Dump entire web pages into the response
- Make claims without source URLs
- Mix speculation with facts
- Exceed 3-5 sources per query (more sources = more context consumed)

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No results found | Try alternative keywords or broader query |
| Page content blocked | Try a different source or official company site |
| Too much content returned | Summarize more aggressively |
| Outdated information | Note the date and flag as potentially stale |

## Example Usage

**User prompt**: "Search the web for Notion pricing and features"

**Expected behavior**:
1. Search: `https://www.bing.com/search?q=Notion+pricing+plans+features+2026`
2. Fetch top 2-3 results (notion.so, review sites)
3. Return summary:
   - Pricing tiers (Free, Plus, Business, Enterprise)
   - Key features per tier
   - Source URLs for each claim

```
