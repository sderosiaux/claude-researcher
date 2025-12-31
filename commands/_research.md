# Research Agent

Autonomous research agent that generates comprehensive, factual reports with citations using Claude Code's native tools.

**Skill:** `~/.claude/skills/researcher.md`

---

## Input

**$ARGUMENTS:** Research query or topic to investigate

---

## Research Configuration

Parse query for modifiers:
- `--depth=shallow|medium|deep` (default: medium)
- `--sources=N` (default: 10, max sources to use)
- `--format=report|outline|resources` (default: report)
- `--tone=objective|analytical|explanatory` (default: objective)
- `--lang=XX` (default: en, output language)

---

## Phase 1: Query Analysis

1. **Decompose the query:**
   - Main topic / entity
   - Subtopics to cover
   - Time relevance (current events? historical? evergreen?)
   - Domain (tech, science, business, general?)

2. **Generate sub-queries** (3-7 based on depth):
   ```
   shallow: 3 sub-queries
   medium: 5 sub-queries
   deep: 7 sub-queries
   ```

3. **Output plan to user** (brief, 3-5 lines max)

---

## Phase 2: Parallel Research

Execute research using Claude Code tools in parallel:

### Web Search (Primary)
```
WebSearch: Execute all sub-queries in parallel
- Use domain-specific search when relevant
- Collect URLs for deeper fetch
```

### Web Fetch (Secondary)
```
WebFetch: Fetch top 3-5 URLs per sub-query
- Extract key facts, quotes, data
- Note source URL for citation
```

### MCP Tools (If relevant)
```
Context7: Technical documentation
claude-mem: Previous research on similar topics
```

### Fallback for blocked sites
If WebFetch returns 403/blocked, use Jina Reader:
```
WebFetch: https://r.jina.ai/{original_url}
```

---

## Phase 3: Synthesis

1. **Aggregate findings:**
   - Group by subtopic
   - Identify consensus vs. contradictions
   - Note data points and statistics

2. **Verify facts:**
   - Cross-reference claims across sources
   - Flag single-source claims
   - Prefer primary sources

3. **Build citation list:**
   - Track all sources used
   - Format: `[Title](URL)`

---

## Phase 4: Report Generation

### Format: `report` (default)

```markdown
# {Topic}

## Executive Summary
[2-3 paragraph overview of key findings]

## Key Findings

### {Subtopic 1}
[Detailed findings with inline citations]

### {Subtopic 2}
[Detailed findings with inline citations]

...

## Analysis
[Synthesis, patterns, implications]

## Conclusion
[Key takeaways, recommendations if applicable]

---

## Sources
- [Source 1](url1)
- [Source 2](url2)
...
```

### Format: `outline`

```markdown
# {Topic} - Research Outline

## Main Themes
1. {Theme 1}
   - Key point A
   - Key point B
2. {Theme 2}
   ...

## Key Sources
- [Source](url) - brief description
...

## Suggested Deep Dives
- {Area needing more research}
```

### Format: `resources`

```markdown
# {Topic} - Resource Collection

## Primary Sources
| Source | Type | Key Value |
|--------|------|-----------|
| [Name](url) | Article/Paper/Doc | Brief description |

## Secondary Sources
...

## Tools & References
...
```

---

## Quality Standards

1. **Accuracy:** Never invent facts. If uncertain, say so.
2. **Citations:** Every factual claim links to source
3. **Balance:** Present multiple viewpoints when they exist
4. **Recency:** Prefer recent sources for evolving topics
5. **Depth:** Match detail level to `--depth` setting

---

## Execution Rules

- **Parallel execution:** Launch all WebSearch queries in single message
- **No user prompts:** Execute autonomously, report results
- **Token efficiency:** Summarize long pages, don't dump raw content
- **Progress updates:** Brief status after each phase

---

## Example Usage

```
/research AI agents in software development --depth=deep --format=report

/research "best practices for API rate limiting" --sources=15

/research climate change impacts 2024 --tone=analytical --lang=fr
```
