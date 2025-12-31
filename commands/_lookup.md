# Quick Lookup Agent

Fast research for simple questions. Single-pass, no multi-agent overhead.

---

## Input

**$ARGUMENTS:** Quick question or lookup query

---

## Execution

### Step 1: Search (Parallel)
```
WebSearch (2-3 queries in parallel):
- "{query}"
- "{query} {current_year}" (if time-sensitive)
```

### Step 2: Fetch Top Results
```
WebFetch: Top 2-3 URLs from search results
- If blocked: try https://r.jina.ai/{url}
```

### Step 3: Synthesize Answer

Provide a concise answer with:
- Direct answer to the question
- Key supporting facts
- Source links

---

## Output Format

```markdown
## Answer

[Direct, concise answer - 1-3 paragraphs]

### Key Facts
- Fact 1
- Fact 2
- Fact 3

### Sources
- [Source 1](url)
- [Source 2](url)
```

---

## When to Use

- Simple factual questions
- Quick definitions
- Current events lookups
- "What is X?" queries
- Price/availability checks

## When NOT to Use

- Complex analysis → use `/research`
- Deep investigation → use `/deep-research`
- Multi-topic comparison → use `/research-team`
