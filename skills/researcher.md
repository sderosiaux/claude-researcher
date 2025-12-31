# Researcher Skill

Comprehensive research skill for Claude Code. Adapted from GPT-Researcher patterns.

---

## Agent Role Selection

Based on the query, adopt the appropriate research persona:

| Domain | Agent | Role |
|--------|-------|------|
| Finance/Investing | Finance Agent | Compose comprehensive, astute, impartial financial reports based on data and trends |
| Business/Market | Business Analyst | Produce insightful, systematically structured business reports based on market trends and strategic analysis |
| Technology/Engineering | Tech Analyst | Create detailed, accurate technical reports with code examples and architecture insights |
| Science/Research | Research Scientist | Generate rigorous, evidence-based scientific reports with proper methodology |
| Travel/Geography | Travel Expert | Draft engaging, insightful travel reports including history, attractions, and cultural insights |
| Health/Medical | Health Analyst | Provide accurate, well-researched health information with proper medical disclaimers |
| Legal | Legal Analyst | Offer detailed legal analysis with appropriate jurisdictional considerations |
| General | Research Assistant | Produce balanced, comprehensive research reports on any topic |

---

## Sub-Query Generation Prompt

```
Write {max_queries} search queries to research the following task: "{query}"

Requirements:
- Queries should form an objective, comprehensive view of the topic
- Each query should explore a different angle or aspect
- Include queries for recent/current information when relevant
- Consider both broad overview and specific detail queries

Current date: {current_date}

Return ONLY a list of queries, one per line.
```

### Query Count by Depth
- shallow: 3 queries
- medium: 5 queries
- deep: 7 queries

---

## Source Curation Prompt

```
Evaluate and curate the following sources for the research task: "{query}"

EVALUATION CRITERIA:
1. Relevance: Include sources directly or partially connected to the query
2. Credibility: Favor authoritative sources but retain others unless clearly untrustworthy
3. Currency: Prefer recent information unless older data is essential
4. Objectivity: Retain sources with bias if they provide unique perspective
5. Quantitative Value: Prioritize sources with statistics, numbers, concrete data

GUIDELINES:
- Include as many relevant sources as possible (up to {max_sources})
- Prioritize sources with numerical data or verifiable facts
- Overlapping content is acceptable if it adds depth
- Exclude sources only if entirely irrelevant or unusable

SOURCES:
{sources}

Return the curated list of sources with brief quality notes.
```

---

## Content Summarization Prompt

```
{content}

Using the above text, summarize it based on the following query: "{query}"

Requirements:
- If the query cannot be directly answered, summarize the text briefly
- Include all factual information: numbers, stats, quotes, dates
- Preserve source attribution for key claims
- Keep summary focused and concise (200-400 words max)
```

---

## Research Report Prompt

```
Information:
"{context}"

---

Using the above information, answer the following query: "{query}" in a detailed report.

REPORT REQUIREMENTS:
- Well-structured, informative, in-depth, and comprehensive
- Include facts, numbers, and data when available
- Minimum {total_words} words
- Markdown syntax with proper headers (# ## ###)
- Use tables for structured data or comparisons

CONTENT GUIDELINES:
- Determine your own concrete opinion based on the information. Do NOT defer to meaningless conclusions.
- Prioritize relevance, reliability, and significance of sources
- Prefer recent articles over older ones when sources are equally trusted
- Do NOT include a table of contents
- Use clear markdown headers to structure the report

CITATION REQUIREMENTS:
- Use in-text citations with hyperlinks: ([source name](url))
- Place citations at the end of sentences/paragraphs that reference them
- Include a "Sources" section at the end with all referenced URLs
- Every URL should be hyperlinked: [Title](url)
- No duplicate sources in the reference list

TONE: {tone}
LANGUAGE: {language}
DATE: {current_date}
```

---

## Resource Report Prompt

```
"{context}"

Based on the above information, generate a bibliography recommendation report for: "{query}"

REQUIREMENTS:
- Detailed analysis of each recommended resource
- Explain how each source contributes to answering the research question
- Focus on relevance, reliability, and significance
- Well-structured with markdown tables
- Include facts, figures, and numbers when available
- Minimum {total_words} words

FORMAT:
| Source | Type | Relevance | Key Insights |
|--------|------|-----------|--------------|
| [Title](url) | Article/Paper/Doc | High/Medium/Low | Brief description |

LANGUAGE: {language}
```

---

## Outline Report Prompt

```
"{context}"

Using the above information, generate an outline for a research report on: "{query}"

REQUIREMENTS:
- Well-structured framework with main sections and subsections
- Key points to be covered in each section
- Logical flow and organization
- Use markdown syntax (## for sections, ### for subsections)
- Include estimated word count per section
- Minimum {total_words} words for the full report

STRUCTURE:
## 1. Section Title
   ### 1.1 Subsection
   - Key point A
   - Key point B
   ### 1.2 Subsection
   ...

LANGUAGE: {language}
```

---

## Deep Research Report Prompt

```
Using the following hierarchically researched information and citations:

"{context}"

Write a comprehensive research report answering: "{query}"

DEEP RESEARCH REQUIREMENTS:
1. Synthesize information from multiple levels of research depth
2. Integrate findings from various research branches
3. Present a coherent narrative from foundational to advanced insights
4. Maintain proper citation of sources throughout
5. Well-structured with clear sections and subsections
6. Minimum {total_words} words
7. Use markdown tables for comparative data and statistics

CONTENT FOCUS:
- Prioritize insights from deeper levels of research
- Highlight connections between different research branches
- Include statistics, data, and concrete examples
- Determine concrete opinions - no meaningless generalizations
- Prioritize reliable, significant sources

CITATION FORMAT:
- In-text: ([source](url)) at end of referencing sentence
- Reference section at end with full URLs

SPECIAL SECTIONS:
- Analysis & Implications
- Contradictions & Debates (where sources disagree)
- Research Limitations
- Methodology summary

TONE: {tone}
LANGUAGE: {language}
DATE: {current_date}
```

---

## Introduction Generation Prompt

```
{research_summary}

Using the above information, prepare a detailed report introduction on: "{query}"

REQUIREMENTS:
- Succinct, well-structured, informative
- Markdown syntax
- Preceded by H1 heading with suitable topic
- In-text citations with hyperlinks
- Do NOT include other sections (body, conclusion, references)
- 2-3 paragraphs maximum

LANGUAGE: {language}
DATE: {current_date}
```

---

## Conclusion Generation Prompt

```
Research task: {query}

Research Report:
{report_content}

Write a concise conclusion that:
1. Recaps the main points of the research
2. Highlights the most important findings
3. Discusses implications or next steps
4. Is 2-3 paragraphs long

Include "## Conclusion" header if not already present.
Use in-text citations with hyperlinks where referencing sources.

LANGUAGE: {language}
```

---

## Subtopic Report Prompt (for detailed/multi-section reports)

```
Context:
"{context}"

Main Topic: {main_topic}
Current Subtopic: {current_subtopic}

CONTENT REQUIREMENTS:
- Focus on the subtopic under the main topic
- Maximum {max_subsections} subsections
- Well-structured, informative, in-depth
- Include facts and numbers when available
- Markdown syntax

UNIQUENESS REQUIREMENTS:
- Review existing headers before writing: {existing_headers}
- Review existing content: {existing_written_contents}
- Do NOT duplicate existing content
- Do NOT reuse existing headers
- Clearly indicate differences when adding similar subsections

STRUCTURE:
- Use ## for subtopic header, ### for subsections
- No introduction or conclusion section
- Include hyperlinks to sources

WORD COUNT: Minimum {total_words} words
TONE: {tone}
LANGUAGE: {language}
DATE: {current_date}
```

---

## Parallel Research Execution Pattern

### Phase 1: Initial Search (Parallel)
```
Launch simultaneously:
- WebSearch: "{query}"
- WebSearch: "{query} overview"
- WebSearch: "{query} {current_year}"
- WebSearch: "{query} guide explained"
```

### Phase 2: Sub-Query Research (Parallel)
```
For each sub-query generated:
- WebSearch: "{sub_query}"
- Collect top URLs for fetching
```

### Phase 3: Content Fetching (Parallel)
```
For top URLs (max 3-5 per sub-query):
- WebFetch: {url}
- If 403/blocked: WebFetch: https://r.jina.ai/{url}
- Summarize immediately (don't store raw content)
```

### Phase 4: Synthesis (Sequential)
```
1. Aggregate all summaries by subtopic
2. Cross-reference facts across sources
3. Build citation list
4. Generate report using appropriate prompt
```

---

## Quality Gates

### Gate 1: Source Quality
- Minimum 3 sources per major claim
- At least 50% of sources from last 2 years (for current topics)
- No single-source critical claims without flagging

### Gate 2: Coverage
- All sub-queries addressed
- No major gaps in topic coverage
- Contradicting viewpoints acknowledged

### Gate 3: Citation Integrity
- Every factual claim has source
- All URLs are valid and accessible
- No broken or placeholder links

### Gate 4: Report Quality
- Meets minimum word count
- Proper markdown structure
- Clear, logical flow
- Concrete conclusions (not vague generalizations)

---

## Error Handling

### WebSearch Fails
- Retry with reformulated query
- Try alternative search terms
- Fall back to broader topic search

### WebFetch Blocked (403)
- Use Jina Reader: `https://r.jina.ai/{url}`
- Skip if still blocked (note in methodology)

### Insufficient Sources
- Expand sub-queries
- Try related topics
- Note limitation in report

### Contradictory Information
- Present both viewpoints
- Note the contradiction explicitly
- Indicate which sources support each view
