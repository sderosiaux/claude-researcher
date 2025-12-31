# Deep Research Agent

Exhaustive multi-level research agent for comprehensive investigation. Uses breadth-first exploration with iterative deepening.

**Skill:** `~/.claude/skills/researcher.md`

---

## Input

**$ARGUMENTS:** Research query requiring exhaustive investigation

---

## Configuration

Parse query for modifiers:
- `--breadth=N` (default: 5, number of parallel branches)
- `--depth=N` (default: 3, levels of sub-query recursion)
- `--max-sources=N` (default: 30)
- `--format=report|outline|resources` (default: report)
- `--focus=comprehensive|technical|business|academic` (default: comprehensive)

---

## Phase 0: Research Planning

1. **Analyze query complexity:**
   - Identify main entity/concept
   - List all dimensions to explore
   - Determine if time-sensitive

2. **Create research tree:**
   ```
   Main Query
   ├── Branch 1: [Aspect A]
   │   ├── Sub-query 1.1
   │   ├── Sub-query 1.2
   │   └── Sub-query 1.3
   ├── Branch 2: [Aspect B]
   │   ├── Sub-query 2.1
   │   └── Sub-query 2.2
   ...
   ```

3. **Output research plan** (show tree structure)

---

## Phase 1: Breadth Search (Level 0)

Execute initial broad search:

```
WebSearch (parallel):
- Main query verbatim
- Main query + "overview"
- Main query + "guide"
- Main query + "explained"
- Main query + current year
```

**Extract from results:**
- Key themes and subtopics
- Major sources to fetch
- Related concepts to explore
- Terminology and jargon

---

## Phase 2: Branch Exploration (Level 1-N)

For each branch in research tree:

### 2.1 Generate Sub-Queries
Based on Level 0 findings, generate specific sub-queries:
```
Example for "AI code assistants":
├── "AI code assistant comparison 2024"
├── "GitHub Copilot vs Cursor vs Claude"
├── "AI coding tools enterprise adoption"
├── "AI code generation accuracy studies"
└── "AI pair programming best practices"
```

### 2.2 Parallel Execution
```
WebSearch: All sub-queries for current level (parallel)
WebFetch: Top 2-3 URLs per sub-query (parallel)
```

### 2.3 Depth Decision
After each level, evaluate:
- Have we found sufficient detail?
- Are there unexplored angles?
- Quality of sources found?

If `current_level < --depth` AND gaps exist:
→ Generate next-level sub-queries
→ Continue to Level N+1

---

## Phase 3: Source Deep Dive

For top sources identified:

### Academic/Technical Sources
```
WebSearch: site:arxiv.org {topic}
WebSearch: site:github.com {topic}
Context7: Check for library documentation
```

### News/Current Events
```
WebSearch: {topic} news {current_month} {current_year}
```

### Expert Opinions
```
WebSearch: {topic} expert analysis
WebSearch: {topic} "according to"
```

---

## Phase 4: Cross-Reference & Verify

1. **Fact Matrix:**
   - List all factual claims
   - Count sources supporting each
   - Flag contradictions

2. **Source Credibility:**
   - Primary vs secondary sources
   - Author/org authority
   - Recency

3. **Gap Analysis:**
   - What questions remain unanswered?
   - What needs primary research?

---

## Phase 5: Comprehensive Report

### Structure

```markdown
# {Topic}: Comprehensive Research Report

**Research Depth:** {breadth} branches x {depth} levels
**Sources Analyzed:** {count}
**Date:** {current_date}

---

## Executive Summary
[3-5 paragraphs covering all major findings]

## Table of Contents
1. [Section 1]
2. [Section 2]
...

---

## 1. {Major Theme 1}

### 1.1 {Subtopic}
[Detailed findings with citations]

### 1.2 {Subtopic}
[Detailed findings with citations]

#### Key Data Points
| Metric | Value | Source |
|--------|-------|--------|
| ... | ... | [ref] |

---

## 2. {Major Theme 2}
...

---

## Analysis & Implications

### Patterns Identified
[Cross-cutting observations]

### Contradictions & Debates
[Where sources disagree]

### Future Outlook
[Trends, predictions from sources]

---

## Research Limitations
- [What couldn't be verified]
- [Areas needing primary research]
- [Potential biases in sources]

---

## Methodology
- Search queries used: {count}
- Pages analyzed: {count}
- Date range of sources: {range}

---

## Complete Source List

### Primary Sources
1. [Title](url) - {brief description}
...

### Secondary Sources
1. [Title](url) - {brief description}
...

### Additional References
...
```

---

## Parallel Execution Strategy

### Level 0 (Initial)
```
Single message with 5 WebSearch calls
```

### Level 1+ (Branches)
```
For each branch:
  - Launch all sub-queries in parallel
  - Wait for completion
  - Fetch top URLs in parallel
  - Synthesize before next level
```

### Resource Management
- Max concurrent WebSearch: 10
- Max concurrent WebFetch: 5
- Summarize long pages immediately (don't store raw)

---

## Quality Gates

Before finalizing report:

1. **Coverage Check:**
   - All branches explored to target depth?
   - Minimum sources per branch met?

2. **Citation Check:**
   - Every fact has source?
   - Sources are accessible?

3. **Balance Check:**
   - Multiple perspectives included?
   - Contradictions acknowledged?

---

## Example Usage

```
/deep-research "The future of autonomous AI agents" --breadth=6 --depth=3

/deep-research "Comparison of vector databases for RAG" --focus=technical --max-sources=40

/deep-research "Impact of EU AI Act on startups" --focus=business
```

---

## Memory Integration

After completing research:
```
Store key findings in claude-mem for future reference:
- Topic: {query}
- Key conclusions
- Best sources found
- Date of research
```
