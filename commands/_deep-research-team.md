# Deep Research Team Agent

Orchestrates `/deep-research` + `/research-team` for exhaustive, verified research.

**Combines:**
- `/deep-research` → breadth + depth exploration (multi-branch, multi-level)
- `/research-team` → verification + quality gates (review cycles)

---

## Input

**$ARGUMENTS:** Complex research query requiring both exhaustive exploration AND rigorous verification

---

## Configuration

Parse query for modifiers (passed to sub-commands):
- `--breadth=N` (default: 4) → passed to /deep-research
- `--depth=N` (default: 3) → passed to /deep-research
- `--agents=N` (default: 3) → passed to /research-team
- `--review-cycles=N` (default: 2) → passed to /research-team
- `--quality=standard|high|academic` (default: high) → passed to /research-team
- `--max-sources=N` (default: 50) → passed to /deep-research

---

## When to Use This Command

| Scenario | Command |
|----------|---------|
| Quick fact check | `/lookup` |
| Standard report | `/research` |
| Explore topic broadly | `/deep-research` |
| High-quality verified report | `/research-team` |
| **Exhaustive + verified research** | **`/deep-research-team`** |

Use when you need BOTH comprehensive coverage AND academic rigor.

---

## Execution Flow

```
/deep-research-team "{query}" --breadth=4 --depth=3 --quality=high
    │
    │  ┌─────────────────────────────────────────┐
    │  │ PHASE 1: Deep Exploration               │
    │  │ Execute /deep-research internally       │
    │  │                                         │
    │  │ • Decompose query into {breadth} branches│
    │  │ • Each branch explores {depth} levels   │
    │  │ • Parallel agents per branch            │
    │  │ • Cross-branch synthesis                │
    │  │                                         │
    │  │ OUTPUT: Comprehensive findings document │
    │  │         with all sources by branch/level│
    │  └─────────────────────────────────────────┘
    │
    ▼
    │  ┌─────────────────────────────────────────┐
    │  │ PHASE 2: Team Verification              │
    │  │ Execute /research-team on Phase 1 output│
    │  │                                         │
    │  │ • Chief Editor structures the report    │
    │  │ • Reviewer agents verify accuracy       │
    │  │ • {review-cycles} revision iterations   │
    │  │ • Quality gates enforced                │
    │  │                                         │
    │  │ OUTPUT: Publication-ready report        │
    │  └─────────────────────────────────────────┘
    │
    ▼
  Final Report (exhaustive + verified)
```

---

## Phase 1: Deep Research

Run the deep-research command with exploration parameters:

```
/deep-research "{query}" --breadth={breadth} --depth={depth} --max-sources={max_sources}
```

This produces:
- Branch-by-branch findings (Level 1 → Level 2 → Level 3)
- Cross-branch synthesis
- Source list organized by depth level
- Gaps identified

**Checkpoint:** Verify deep-research produced comprehensive findings before proceeding.

---

## Phase 2: Team Verification

Feed Phase 1 findings into research-team for verification:

```
/research-team "Verify and structure these findings into a publication-ready report:

{deep_research_output}

Original query: {query}"

--agents={agents} --review-cycles={review_cycles} --quality={quality}
```

This produces:
- Structured report with proper sections
- Reviewer feedback incorporated
- Quality score meets threshold
- Publication-ready formatting

---

## Quality Thresholds (from /research-team)

| Quality Level | Min Sources | Min Words | Review Score | Expert Sources |
|---------------|-------------|-----------|--------------|----------------|
| standard | 30 | 4000 | 7/10 | 20% |
| high | 50 | 6000 | 8/10 | 30% |
| academic | 80 | 10000 | 9/10 | 50% |

---

## Output Structure

```
.research/{query_slug}/
├── phase-1-deep-research/     # From /deep-research
│   ├── branches/
│   ├── synthesis.md
│   └── sources.md
├── phase-2-verification/      # From /research-team
│   ├── reviews/
│   └── revisions/
└── final-report.md            # Publication-ready
```

---

## Example Usage

```
/deep-research-team "The future of AI agents in enterprise software" --quality=academic

/deep-research-team "Comparison of RAG architectures 2024" --breadth=5 --depth=3

/deep-research-team "Impact of EU AI Act on startups" --review-cycles=3 --quality=high
```

---

## Comparison

| Aspect | `/deep-research` | `/research-team` | `/deep-research-team` |
|--------|------------------|------------------|----------------------|
| Exploration | ✅ Multi-level | ❌ Single-level | ✅ Multi-level |
| Verification | ❌ None | ✅ Review cycles | ✅ Review cycles |
| Quality gates | ❌ None | ✅ Enforced | ✅ Enforced |
| Time | ~10 min | ~15 min | ~25-35 min |
| Use case | Explore broadly | Verified report | **Both** |

This command is the composition: `deep-research-team = deep-research ∘ research-team`
