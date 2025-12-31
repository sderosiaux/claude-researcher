# Research Team Agent

Multi-agent collaborative research system. Orchestrates specialized agents working in parallel with quality gates.

**Skill:** `~/.claude/skills/researcher.md`
**Knowledge:** `~/.claude/knowledge/_orchestration/`

---

## Input

**$ARGUMENTS:** Research query requiring comprehensive team investigation

---

## Configuration

Parse query for modifiers:
- `--agents=N` (default: 3, parallel research agents)
- `--review-cycles=N` (default: 2, revision iterations)
- `--min-sources=N` (default: 20)
- `--format=report|paper|brief` (default: report)
- `--quality=standard|high|academic` (default: standard)

---

## Agent Roles

### Chief Editor (Orchestrator - YOU)
- Plans research outline
- Assigns topics to Research Agents
- Coordinates workflow
- Enforces quality gates
- Produces final report

### Research Agents (Parallel Task Agents)
- Conduct research on assigned subtopics
- Gather sources and evidence
- Write draft sections
- Return findings to Chief Editor

### Reviewer Agent (Task Agent)
- Reviews draft for quality
- Checks citation accuracy
- Identifies gaps and inconsistencies
- Provides revision feedback

### Reviser Agent (Task Agent)
- Incorporates reviewer feedback
- Strengthens weak sections
- Adds missing information
- Improves clarity and flow

### Publisher Agent (Final Pass)
- Formats final report
- Ensures consistent style
- Validates all links
- Generates bibliography

---

## Workflow

```
Chief Editor
    │
    ├─[Phase 1: Planning]──────────────────────────────────────┐
    │   Create research outline with subtopics                 │
    │   Gate G0: Outline approved                              │
    │                                                          │
    ├─[Phase 2: Research]──────────────────────────────────────┤
    │   Launch N Research Agents in PARALLEL                   │
    │   Each agent researches assigned subtopic                │
    │   Gate G1: All agents return findings                    │
    │                                                          │
    ├─[Phase 3: Draft Assembly]────────────────────────────────┤
    │   Combine findings into draft report                     │
    │   Gate G2: Draft meets minimum word count                │
    │                                                          │
    ├─[Phase 4: Review Cycle]──────────────────────────────────┤
    │   Reviewer Agent evaluates draft                         │
    │   Gate G3: Quality score >= threshold                    │
    │   If fails: Reviser Agent improves, repeat               │
    │                                                          │
    └─[Phase 5: Publication]───────────────────────────────────┘
        Publisher Agent formats final report
        Gate G4: All citations valid, structure complete
```

---

## Phase 0: Mission Analysis

1. **Analyze query:**
   - Main topic identification
   - Scope determination
   - Time sensitivity check

2. **Generate research outline:**
   ```markdown
   # Research Outline: {query}

   ## Subtopics to Research
   1. {Subtopic A} - Assigned to Agent 1
   2. {Subtopic B} - Assigned to Agent 2
   3. {Subtopic C} - Assigned to Agent 3
   ...

   ## Research Focus Areas
   - Key questions to answer
   - Data types needed
   - Source priorities
   ```

3. **Define quality thresholds:**
   | Quality Level | Min Sources | Min Words | Review Score |
   |---------------|-------------|-----------|--------------|
   | standard | 15 | 2000 | 7/10 |
   | high | 25 | 4000 | 8/10 |
   | academic | 40 | 6000 | 9/10 |

**GATE G0:** Outline created with clear subtopic assignments.

---

## Phase 1: Parallel Research

Launch Research Agents using Task tool with `run_in_background: true`:

```
For each subtopic (in parallel):
  Task Agent (subagent_type: "general-purpose"):
    prompt: """
    You are a Research Agent investigating: "{subtopic}"

    Main query context: "{main_query}"

    INSTRUCTIONS:
    1. Use WebSearch to find 5-10 relevant sources
    2. Use WebFetch to extract key information from top sources
    3. Summarize findings with full citations
    4. Return structured findings:

    ## Findings: {subtopic}

    ### Key Points
    - Point 1 ([source](url))
    - Point 2 ([source](url))

    ### Data & Statistics
    - Stat 1 (source)
    - Stat 2 (source)

    ### Sources Used
    1. [Title](url) - brief description
    2. [Title](url) - brief description

    ### Gaps Identified
    - What couldn't be found
    """
```

Wait for all agents to complete using AgentOutputTool.

**GATE G1:** All research agents returned findings. Minimum sources met per subtopic.

---

## Phase 2: Draft Assembly

Combine all agent findings into cohesive draft:

1. **Merge findings by subtopic**
2. **Remove duplicates**
3. **Identify cross-topic connections**
4. **Write transitions between sections**
5. **Add introduction and preliminary conclusion**

Draft structure:
```markdown
# {Main Topic}

## Executive Summary
[Brief overview of findings]

## 1. {Subtopic A}
[Agent 1 findings, edited for flow]

## 2. {Subtopic B}
[Agent 2 findings, edited for flow]

## 3. {Subtopic C}
[Agent 3 findings, edited for flow]

## Analysis
[Cross-topic insights]

## Preliminary Conclusion
[Initial takeaways]

## Sources
[Combined, deduplicated source list]
```

**GATE G2:** Draft assembled. Word count >= minimum. All subtopics covered.

---

## Phase 3: Review Cycle

### Reviewer Agent Task

```
Task Agent (subagent_type: "code-reviewer" or "general-purpose"):
  prompt: """
  You are a Research Reviewer. Evaluate this draft report:

  {draft_content}

  REVIEW CRITERIA:
  1. Accuracy: Are claims supported by cited sources?
  2. Completeness: Are there gaps in coverage?
  3. Clarity: Is the writing clear and well-organized?
  4. Citations: Are sources properly attributed?
  5. Balance: Are multiple perspectives represented?
  6. Depth: Is the analysis substantive?

  RETURN:
  ## Review Score: X/10

  ## Strengths
  - ...

  ## Issues Found
  1. [CRITICAL] Issue description
  2. [MAJOR] Issue description
  3. [MINOR] Issue description

  ## Specific Revisions Needed
  - Section X: Add more data on...
  - Section Y: Citation missing for claim...
  - Section Z: Clarify explanation of...

  ## Missing Information
  - Topics not covered: ...
  - Questions unanswered: ...
  """
```

**GATE G3:** Review score >= threshold for quality level.

If GATE G3 fails:

### Reviser Agent Task

```
Task Agent (subagent_type: "general-purpose"):
  prompt: """
  You are a Research Reviser. Improve this draft based on reviewer feedback:

  ORIGINAL DRAFT:
  {draft_content}

  REVIEWER FEEDBACK:
  {review_feedback}

  INSTRUCTIONS:
  1. Address all CRITICAL and MAJOR issues
  2. Improve sections flagged for revision
  3. Add missing information using WebSearch/WebFetch
  4. Strengthen citations where needed
  5. Maintain consistent tone and style

  Return the COMPLETE revised report, not just changes.
  """
```

Repeat review cycle up to `--review-cycles` times.

---

## Phase 4: Publication

### Publisher Agent Task

```
Task Agent (subagent_type: "general-purpose"):
  prompt: """
  You are the Publisher. Finalize this research report:

  {revised_content}

  FINAL CHECKS:
  1. Verify all URLs are valid (test each link mentally)
  2. Ensure consistent formatting throughout
  3. Check header hierarchy (# ## ###)
  4. Deduplicate source list
  5. Add methodology section if missing
  6. Verify word count meets target

  FORMAT REQUIREMENTS:
  - Clean markdown structure
  - Proper citation format: ([Source](url))
  - Table of contents for reports > 3000 words
  - Clear section breaks
  - Professional tone

  ADDITIONS:
  - Add "Research Methodology" section
  - Add "Limitations" section
  - Ensure "Sources" section is complete

  Return the FINAL publication-ready report.
  """
```

**GATE G4:** All links valid. Structure complete. Final quality check passed.

---

## Output Structure

```
.research/{query_slug}/
├── 00-outline.md        # Research plan
├── 01-findings/         # Individual agent findings
│   ├── agent-1.md
│   ├── agent-2.md
│   └── agent-3.md
├── 02-draft.md          # Combined draft
├── 03-reviews/          # Review feedback
│   ├── review-1.md
│   └── review-2.md
├── 04-revisions/        # Revised versions
│   ├── revision-1.md
│   └── revision-2.md
└── 05-final-report.md   # Publication-ready report
```

---

## Parallel Execution Rules

### Parallelize
- All research agents (Phase 1)
- Independent source fetching within agents

### Sequential
- Review after draft assembly
- Revision after review
- Publication after final review passes

### Agent Coordination
```
Single message with multiple Task tool calls:
  - Task 1: Research Agent for Subtopic A
  - Task 2: Research Agent for Subtopic B
  - Task 3: Research Agent for Subtopic C

Wait for all with AgentOutputTool (blocking)
```

---

## Quality Thresholds

### Standard Quality
- Review score: 7/10
- Max review cycles: 2
- Min sources: 15
- Focus: Coverage and accuracy

### High Quality
- Review score: 8/10
- Max review cycles: 3
- Min sources: 25
- Focus: Depth and analysis

### Academic Quality
- Review score: 9/10
- Max review cycles: 4
- Min sources: 40
- Focus: Rigor and citations

---

## Error Handling

### Agent Fails to Return
- Set timeout (5 minutes per agent)
- If timeout: proceed with available findings
- Note gap in methodology section

### Insufficient Sources
- Expand search queries
- Try alternative subtopic formulations
- Document limitation

### Review Cycle Stuck
- After max cycles, proceed with best version
- Note quality concerns in limitations
- Flag for manual review

---

## Example Usage

```
/research-team "Impact of AI on software development jobs" --agents=4 --quality=high

/research-team "Comparison of cloud database solutions 2024" --format=brief --min-sources=30

/research-team "Climate policy effectiveness across G20 nations" --quality=academic --review-cycles=4
```

---

## Memory Integration

After completing research:
```
Store in claude-mem:
- Query: {original_query}
- Key findings summary
- Best sources discovered
- Research date
- Quality score achieved
```

This enables future research to build on previous work.
