# Deep Research Team Agent

Exhaustive multi-level research with multi-agent collaboration, quality gates, and iterative refinement. Combines the depth of `/deep-research` with the quality control of `/research-team`.

**Skill:** `~/.claude/skills/researcher.md`

---

## Input

**$ARGUMENTS:** Complex research query requiring both exhaustive exploration AND rigorous verification

---

## Configuration

Parse query for modifiers:
- `--breadth=N` (default: 4, parallel research branches)
- `--depth=N` (default: 3, levels of recursive exploration per branch)
- `--agents=N` (default: 4, parallel research agents per level)
- `--review-cycles=N` (default: 3, revision iterations)
- `--quality=standard|high|academic` (default: high)
- `--max-sources=N` (default: 50)

---

## When to Use This Command

| Scenario | Command |
|----------|---------|
| Quick fact check | `/lookup` |
| Standard report | `/research` |
| Explore topic broadly | `/deep-research` |
| High-quality verified report | `/research-team` |
| **Exhaustive + verified research** | **`/deep-research-team`** |

Use `/deep-research-team` when you need:
- Comprehensive coverage (explore all angles)
- Academic rigor (multiple verification passes)
- Authoritative output (suitable for publication)
- Complex topics with many facets

---

## Phase 0: Research Architecture

### 0.1 Query Decomposition

Analyze the query to build a research tree:

```
Main Query: "{query}"
    │
    ├── Branch 1: [Dimension A]
    │     ├── Level 1: Overview research
    │     ├── Level 2: Detailed investigation
    │     └── Level 3: Expert/academic sources
    │
    ├── Branch 2: [Dimension B]
    │     ├── Level 1: Overview research
    │     ├── Level 2: Detailed investigation
    │     └── Level 3: Expert/academic sources
    │
    ├── Branch 3: [Dimension C]
    │     └── ...
    │
    └── Branch N: [Dimension N]
          └── ...
```

### 0.2 Quality Thresholds

| Quality Level | Min Sources | Min Words | Review Score | Expert Sources |
|---------------|-------------|-----------|--------------|----------------|
| standard | 30 | 4000 | 7/10 | 20% |
| high | 50 | 6000 | 8/10 | 30% |
| academic | 80 | 10000 | 9/10 | 50% |

**GATE G0:** Research tree defined with clear branches and levels.

---

## Phase 1: Branch Research (Parallel + Deep)

For EACH branch, launch a dedicated Research Agent that performs multi-level exploration:

### Branch Agent Task

```
Task Agent (subagent_type: "general-purpose", run_in_background: true):
  prompt: """
  You are a Deep Research Agent investigating Branch: "{branch_name}"

  Main query context: "{main_query}"

  Your task: Conduct {depth}-level deep research on this dimension.

  LEVEL 1 - BREADTH (Overview):
  1. WebSearch: "{branch_topic} overview"
  2. WebSearch: "{branch_topic} {current_year}"
  3. WebSearch: "{branch_topic} explained guide"
  4. Extract key themes, terminology, major players
  5. Identify sub-topics for Level 2

  LEVEL 2 - DEPTH (Detailed Investigation):
  For each sub-topic from Level 1:
  1. WebSearch: specific queries for each sub-topic
  2. WebFetch: Top 3 URLs per sub-topic
  3. Extract data, statistics, case studies
  4. Identify expert sources for Level 3

  LEVEL 3 - EXPERTISE (Academic/Authoritative):
  1. WebSearch: site:arxiv.org "{branch_topic}"
  2. WebSearch: site:github.com "{branch_topic}"
  3. WebSearch: "{branch_topic}" research paper
  4. WebSearch: "{branch_topic}" expert analysis
  5. Context7: Check for technical documentation

  RETURN FORMAT:
  ## Branch: {branch_name}

  ### Level 1 Findings (Overview)
  - Theme 1: [description] ([source](url))
  - Theme 2: [description] ([source](url))

  ### Level 2 Findings (Detailed)
  #### Sub-topic A
  - Finding 1 with data ([source](url))
  - Finding 2 with stats ([source](url))

  #### Sub-topic B
  - ...

  ### Level 3 Findings (Expert)
  - Academic source 1: [key insight] ([paper](url))
  - Expert analysis: [insight] ([source](url))

  ### Sources by Level
  | Level | Count | Quality |
  |-------|-------|---------|
  | 1 (Overview) | N | General |
  | 2 (Detailed) | N | Specialized |
  | 3 (Expert) | N | Academic/Authoritative |

  ### Gaps Identified
  - Questions still unanswered
  - Areas needing more investigation
  """
```

Launch ALL branch agents in parallel. Wait for completion with AgentOutputTool.

**GATE G1:** All branch agents completed. Minimum sources per branch met. All levels explored.

---

## Phase 2: Cross-Branch Synthesis

After all branches complete:

### 2.1 Aggregate Findings

Combine all branch findings into structured context:
- Group overlapping findings
- Identify contradictions between branches
- Map connections across dimensions
- Note consensus vs. disagreement

### 2.2 Gap Analysis

```
Synthesis Agent Task:
  prompt: """
  Review all branch findings and identify:

  1. OVERLAPS: Where do multiple branches agree?
  2. CONTRADICTIONS: Where do sources disagree?
  3. GAPS: What questions remain unanswered?
  4. CONNECTIONS: What patterns span branches?

  For each gap, generate additional research queries.
  """
```

### 2.3 Fill Gaps (if needed)

If significant gaps exist, launch targeted research agents:
```
For each gap:
  Task Agent: Focused research on specific gap
```

**GATE G2:** All major gaps addressed. Cross-branch synthesis complete.

---

## Phase 3: Draft Assembly

Assemble comprehensive draft from synthesized findings:

### Draft Structure

```markdown
# {Topic}: Comprehensive Research Report

**Research Depth:** {breadth} branches x {depth} levels
**Sources Analyzed:** {count}
**Expert Sources:** {count} ({percentage}%)
**Date:** {current_date}

---

## Executive Summary
[3-5 paragraphs synthesizing key findings across all branches]

## Table of Contents
[Auto-generated from sections]

---

## 1. {Branch 1 Topic}

### 1.1 Overview
[Level 1 findings]

### 1.2 Detailed Analysis
[Level 2 findings with data]

### 1.3 Expert Perspectives
[Level 3 findings with academic sources]

---

## 2. {Branch 2 Topic}
[Same structure]

---

## N. {Branch N Topic}
[Same structure]

---

## Cross-Cutting Analysis

### Patterns Identified
[Connections across branches]

### Points of Consensus
[Where sources agree]

### Areas of Debate
[Where sources disagree, with both perspectives]

---

## Implications & Recommendations
[Synthesis of findings into actionable insights]

---

## Research Methodology
- Branches explored: {list}
- Depth levels: {count}
- Total queries: {count}
- Sources analyzed: {count}
- Expert sources: {count}

---

## Limitations
[What couldn't be verified, gaps remaining]

---

## Complete Source List
[Organized by branch and level]
```

**GATE G3:** Draft assembled. Word count >= minimum. All branches represented.

---

## Phase 4: Multi-Cycle Review

### Cycle Structure

Each review cycle involves:

1. **Expert Reviewer** - Domain accuracy
2. **Structure Reviewer** - Organization and flow
3. **Citation Reviewer** - Source verification
4. **Reviser** - Incorporates all feedback

### 4.1 Expert Review

```
Task Agent (subagent_type: "general-purpose"):
  prompt: """
  You are a Domain Expert Reviewer. Evaluate this research report:

  {draft_content}

  EVALUATE:
  1. Are claims accurately representing the sources?
  2. Are expert sources properly weighted?
  3. Is the analysis substantive or superficial?
  4. Are conclusions well-supported?
  5. Are counterarguments addressed?

  RETURN:
  ## Expert Review Score: X/10

  ## Accuracy Issues
  - [CRITICAL] Claim X misrepresents source
  - [MAJOR] Section Y oversimplifies
  - [MINOR] Nuance missing in Z

  ## Depth Assessment
  - Sections needing more depth: ...
  - Well-analyzed sections: ...

  ## Expert Verdict
  [1-2 paragraphs on overall quality]
  """
```

### 4.2 Structure Review

```
Task Agent:
  prompt: """
  Evaluate the report structure:

  1. Is the flow logical?
  2. Are sections balanced?
  3. Is cross-referencing effective?
  4. Is the executive summary accurate?

  Score: X/10
  Structure improvements: ...
  """
```

### 4.3 Citation Review

```
Task Agent:
  prompt: """
  Verify citations:

  1. Every claim has a source?
  2. Sources are properly attributed?
  3. Expert sources are correctly weighted?
  4. No broken or placeholder links?

  Score: X/10
  Citation issues: ...
  """
```

### 4.4 Revision

```
Task Agent:
  prompt: """
  Revise the draft incorporating:

  EXPERT FEEDBACK:
  {expert_review}

  STRUCTURE FEEDBACK:
  {structure_review}

  CITATION FEEDBACK:
  {citation_review}

  Return COMPLETE revised report.
  """
```

### 4.5 Re-Review

After revision, calculate aggregate score:
```
Aggregate Score = (Expert + Structure + Citation) / 3
```

If score < threshold: repeat cycle (up to max cycles)

**GATE G4:** Aggregate score >= quality threshold.

---

## Phase 5: Final Publication

### Publisher Agent

```
Task Agent:
  prompt: """
  Finalize this research report:

  {revised_content}

  FINAL CHECKS:
  1. Consistent formatting throughout
  2. All URLs valid
  3. Header hierarchy correct
  4. Source list deduplicated
  5. Word count verified
  6. Expert source percentage verified

  ADDITIONS:
  - Ensure methodology section complete
  - Add research timeline
  - Verify limitations are honest

  Return FINAL publication-ready report.
  """
```

**GATE G5:** All checks passed. Report publication-ready.

---

## Output Structure

```
.research/{query_slug}/
├── 00-research-tree.md       # Branch/level structure
├── 01-branch-findings/       # Per-branch deep research
│   ├── branch-1.md
│   ├── branch-2.md
│   └── branch-N.md
├── 02-synthesis.md           # Cross-branch analysis
├── 03-draft.md               # Assembled draft
├── 04-reviews/               # Review cycles
│   ├── cycle-1/
│   │   ├── expert.md
│   │   ├── structure.md
│   │   ├── citation.md
│   │   └── revision.md
│   ├── cycle-2/
│   └── cycle-N/
├── 05-final-report.md        # Publication-ready
└── 06-metadata.md            # Stats, scores, timeline
```

---

## Execution Timeline

```
Phase 0: Research Architecture     [~2 min]
    │
Phase 1: Branch Research (parallel) [~10-15 min]
    │     └── {breadth} agents x {depth} levels each
    │
Phase 2: Cross-Branch Synthesis     [~3 min]
    │
Phase 3: Draft Assembly             [~5 min]
    │
Phase 4: Review Cycles              [~5 min per cycle]
    │     └── {review_cycles} iterations
    │
Phase 5: Final Publication          [~2 min]
    │
TOTAL: ~30-45 minutes for high-quality research
```

---

## Example Usage

```
/deep-research-team "The future of AI agents in enterprise software" --quality=academic

/deep-research-team "Comparison of RAG architectures 2024" --breadth=5 --depth=3

/deep-research-team "Impact of EU AI Act on startups" --review-cycles=4 --max-sources=80
```

---

## Memory Integration

After completion:
```
Store in claude-mem:
- Query: {original_query}
- Research tree structure
- Key findings per branch
- Best sources discovered
- Quality scores achieved
- Research timestamp
```

Enables future research to build on comprehensive prior work.

---

## Comparison with Other Commands

| Aspect | `/deep-research` | `/research-team` | `/deep-research-team` |
|--------|------------------|------------------|----------------------|
| Breadth | High | Medium | High |
| Depth | High (recursive) | Medium | High (recursive) |
| Verification | None | Review cycles | Multiple reviewers |
| Quality gates | None | 1 gate | 5 gates |
| Time | ~10 min | ~15 min | ~30-45 min |
| Use case | Explore broadly | Verified report | Authoritative research |
