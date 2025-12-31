# Deep Research Team Agent

Orchestrates `/deep-research` → parallel `/research-team` instances for exhaustive, verified research.

**Flow:**
1. `/deep-research` → discovers branches (dimensions of the topic)
2. **Parallel** `/research-team` → one per branch (verified deep-dive)
3. Synthesis → merge verified reports into final output

---

## Input

**$ARGUMENTS:** Complex research query requiring both exhaustive exploration AND rigorous verification

---

## Configuration

Parse query for modifiers (passed to sub-commands):
- `--breadth=N` (default: 4) → number of branches from /deep-research
- `--depth=N` (default: 2) → depth levels in /deep-research exploration
- `--agents=N` (default: 3) → agents per /research-team instance
- `--review-cycles=N` (default: 2) → review iterations per branch
- `--quality=standard|high|academic` (default: high) → quality threshold
- `--max-sources=N` (default: 50) → total sources across all branches

---

## When to Use This Command

| Scenario | Command |
|----------|---------|
| Quick fact check | `/lookup` |
| Standard report | `/research` |
| Explore topic broadly | `/deep-research` |
| High-quality verified report | `/research-team` |
| **Exhaustive + verified research** | **`/deep-research-team`** |

Use when you need BOTH comprehensive coverage AND academic rigor on each dimension.

---

## Execution Flow

```
/deep-research-team "{query}" --breadth=4 --quality=high
    │
    │  ┌─────────────────────────────────────────┐
    │  │ PHASE 1: Branch Discovery               │
    │  │ Execute /deep-research (lightweight)    │
    │  │                                         │
    │  │ • Decompose query into {breadth} branches│
    │  │ • Initial exploration per branch        │
    │  │ • Identify key dimensions               │
    │  │                                         │
    │  │ OUTPUT: Branch list with context        │
    │  │   Branch 1: "Technical challenges"      │
    │  │   Branch 2: "Business implications"     │
    │  │   Branch 3: "Case studies"              │
    │  │   Branch 4: "Future outlook"            │
    │  └─────────────────────────────────────────┘
    │
    ▼
    │  ┌─────────────────────────────────────────┐
    │  │ PHASE 2: Parallel Verified Research     │
    │  │ Launch /research-team per branch        │
    │  │                                         │
    │  │ [PARALLEL]                              │
    │  │ ├── /research-team "Branch 1: ..."      │
    │  │ ├── /research-team "Branch 2: ..."      │
    │  │ ├── /research-team "Branch 3: ..."      │
    │  │ └── /research-team "Branch 4: ..."      │
    │  │                                         │
    │  │ Each instance:                          │
    │  │ • Full research cycle                   │
    │  │ • Reviewer verification                 │
    │  │ • Quality gates enforced                │
    │  │                                         │
    │  │ OUTPUT: Verified report per branch      │
    │  └─────────────────────────────────────────┘
    │
    ▼
    │  ┌─────────────────────────────────────────┐
    │  │ PHASE 3: Synthesis                      │
    │  │ Merge verified branch reports           │
    │  │                                         │
    │  │ • Cross-branch patterns                 │
    │  │ • Contradictions resolved               │
    │  │ • Executive summary                     │
    │  │ • Final quality check                   │
    │  │                                         │
    │  │ OUTPUT: Publication-ready report        │
    │  └─────────────────────────────────────────┘
    │
    ▼
  Final Report (exhaustive + verified per branch)
```

---

## Phase 1: Branch Discovery

Run `/deep-research` in lightweight mode to discover dimensions:

```
/deep-research "{query}" --breadth={breadth} --depth=1 --max-sources=10
```

**Goal:** Identify the key branches, not exhaustive research yet.

**Output format expected:**
```markdown
## Research Branches Identified

### Branch 1: [Title]
Context: [Brief description of this dimension]
Key questions: [What this branch should answer]

### Branch 2: [Title]
...
```

**Checkpoint:** Verify branches are distinct and comprehensive before proceeding.

---

## Phase 2: Parallel Research Teams

For EACH branch from Phase 1, launch a `/research-team` instance:

```
[PARALLEL - Launch all simultaneously using Task agents with run_in_background]

Task Agent 1:
  /research-team "In-depth research on {Branch 1 Title}

  Context: {Branch 1 description}
  Main query: {original query}
  Focus: {key questions for this branch}"

  --agents={agents} --review-cycles={review_cycles} --quality={quality}

Task Agent 2:
  /research-team "In-depth research on {Branch 2 Title}
  ..."

Task Agent N:
  /research-team "In-depth research on {Branch N Title}
  ..."
```

**Each /research-team instance produces:**
- Verified report for that branch
- Sources validated
- Quality score meeting threshold
- Review feedback incorporated

**Wait for all agents:** Use AgentOutputTool to collect all branch reports.

---

## Phase 3: Synthesis

Merge all verified branch reports into final comprehensive report:

```markdown
# {Topic}: Comprehensive Research Report

**Methodology:** {breadth} branches x /research-team verification
**Total Sources:** {sum across branches}
**Quality Level:** {quality}
**Date:** {current_date}

---

## Executive Summary
[Synthesize key findings across ALL branches - 3-5 paragraphs]

---

## Branch 1: {Title}
[Full verified report from /research-team instance 1]

---

## Branch 2: {Title}
[Full verified report from /research-team instance 2]

---

## Branch N: {Title}
[Full verified report from /research-team instance N]

---

## Cross-Branch Analysis

### Patterns Across Branches
[What themes appeared in multiple branches?]

### Points of Consensus
[Where do all branches agree?]

### Contradictions & Tensions
[Where do branches disagree? Both perspectives presented.]

### Synthesis
[How do the branches connect? Unified understanding.]

---

## Implications & Recommendations
[Actionable insights from the combined research]

---

## Methodology
- Branches explored: {list}
- Research team instances: {count}
- Review cycles per branch: {count}
- Total sources: {count}
- Quality threshold: {level}

---

## Complete Source List
[Organized by branch]
```

---

## Quality Thresholds (per branch)

| Quality Level | Min Sources/Branch | Min Words/Branch | Review Score |
|---------------|-------------------|------------------|--------------|
| standard | 10 | 1500 | 7/10 |
| high | 15 | 2500 | 8/10 |
| academic | 25 | 4000 | 9/10 |

---

## Output Structure

```
.research/{query_slug}/
├── phase-1-branches.md           # Branch discovery output
├── phase-2-branch-reports/       # From parallel /research-team
│   ├── branch-1-report.md        # Verified
│   ├── branch-2-report.md        # Verified
│   └── branch-N-report.md        # Verified
├── phase-3-synthesis.md          # Cross-branch analysis
└── final-report.md               # Publication-ready
```

---

## Example Usage

```
/deep-research-team "The future of AI agents in enterprise software" --quality=academic

/deep-research-team "Comparison of RAG architectures 2024" --breadth=5

/deep-research-team "Impact of EU AI Act on startups" --breadth=4 --review-cycles=3
```

---

## Comparison

| Aspect | `/deep-research` | `/research-team` | `/deep-research-team` |
|--------|------------------|------------------|----------------------|
| Branches | ✅ Multiple | ❌ Single topic | ✅ Multiple |
| Per-branch verification | ❌ None | ✅ Yes | ✅ Yes (parallel) |
| Quality gates | ❌ None | ✅ Per report | ✅ Per branch |
| Parallelism | Branches | Agents | **Branches × Teams** |
| Time | ~10 min | ~15 min | ~20-30 min |

**Composition:** `deep-research-team = deep-research.branches.map(research-team) + synthesis`
