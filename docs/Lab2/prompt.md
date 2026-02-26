# Lab 2 Prompt — Customer Needs Synthesis

You are a senior product research analyst. Your task is to produce a **Customer Needs Document** for Worklane, a B2B product planning platform, by systematically processing a large corpus of customer artifacts.

## Context

Worklane is used by multi-team organizations for roadmap planning, intake management, and portfolio visibility. Customers include enterprise PMOs, business unit program managers, IT/identity teams, and executive sponsors. The artifact corpus spans 12 months of customer feedback across 10 artifact types (~1,780 files). This corpus is too large for a single context window, so you MUST process it in batches.

## Step-by-step instructions

### Phase 1 — Orientation
1. Read all files in `docs/Lab2/context/seed/` (01 through 05) to understand the company, product, methodology, stakeholders, and deliverable format.
2. Read `docs/Lab2/context/00-index.md` to understand the corpus layout.

### Phase 2 — Batch processing of artifacts
Process each of the 10 folders below **in order**. For each folder, process files in chunks of the specified size. After **every chunk**, update the notes file `docs/Lab2/processing-notes.md` with:
- Which files you just read (range)
- New needs discovered (with quoted evidence and filename)
- Updated mention counts for previously seen needs
- Any noise files skipped (positive sentiment / off-topic / no actionable pain)

**Folder processing plan:**

| # | Folder | File count | Chunk size | Chunks |
|---|--------|-----------|------------|--------|
| 1 | `customer-interviews` | 150 | 25 | 6 |
| 2 | `support-tickets` | 500 | 50 | 10 |
| 3 | `sales-call-notes` | 150 | 25 | 6 |
| 4 | `nps-survey-responses` | 400 | 50 | 8 |
| 5 | `feature-requests` | 200 | 25 | 8 |
| 6 | `community-forum-posts` | 100 | 25 | 4 |
| 7 | `cs-qbr-notes` | 80 | 20 | 4 |
| 8 | `churn-exit-interviews` | 80 | 20 | 4 |
| 9 | `internal-meeting-notes` | 80 | 20 | 4 |
| 10 | `cab-session-transcripts` | 40 | 20 | 2 |

File naming pattern: `{folder-name}/{folder-name}-{NNN}.md` (e.g., `customer-interviews/customer-interviews-001.md`).

Base path for all generated artifacts: `docs/Lab2/context/generated/`

**For each chunk:**
1. Read all files in the chunk range.
2. For each file, extract any customer pain point, unmet need, or friction signal. Record the need theme, a representative quote, and the filename.
3. Ignore noise files (files with only positive sentiment, off-topic commentary, or no actionable pain signal).
4. Map extracted signals to your running deduplicated needs list. If a signal matches an existing need, increment its count and optionally add a new evidence snippet if it's from a new artifact type. If it's genuinely new, add it as a new need.
5. **Write/append** the updated running summary to `docs/Lab2/processing-notes.md`. Include: chunk ID, files processed, new needs found, updated cumulative counts.

### Phase 3 — Synthesis
After ALL 56 chunks across ALL 10 folders are processed:
1. Re-read `docs/Lab2/processing-notes.md` in full.
2. Deduplicate and normalize need names (merge synonyms, align wording).
3. Assign each need to a frequency band:
   - **Common**: 25+ mentions
   - **Medium**: 10–24 mentions
   - **Rare**: <10 mentions
4. For each need, select 1–2 representative evidence excerpts drawn from different artifact types when possible.
5. Identify affected customer segments (Enterprise PMO, BU Program Managers, IT/Identity, Executive Sponsors).
6. Assess business impact if unresolved (churn risk, expansion blocker, support cost, competitive gap).

### Phase 4 — Final output
Write the final document to `docs/Lab2/CustomerNeeds-Output.md` using this exact structure:

```
# Customer Needs Document — Worklane

## Summary
[2-3 sentence executive overview of key themes]

## Ranked Customer Needs

### [Rank]. [Need Name]
- **Frequency**: [Common / Medium / Rare] (~N mentions across corpus)
- **Affected segments**: [list]
- **Evidence**:
  - "[quote]" — *source-filename.md*
  - "[quote]" — *source-filename.md*
- **Business impact if unresolved**: [1-2 sentences]

[Repeat for all discovered needs, ranked by mention count descending]

## Methodology
[Brief description of processing approach: chunk sizes, note-saving cadence, deduplication strategy]
```

## Hard rules
- Do NOT produce a PRD, solution design, or implementation plan.
- Do NOT skip any folder or file range — every file must be read.
- Do NOT synthesize the final document until all chunks from all folders are processed.
- Keep the notes file updated after every chunk — this is your working memory.
- Deduplicate needs carefully: different wording for the same underlying pain should be merged.
- Preserve filename-level evidence traceability.
