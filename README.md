# Claude Researcher

**Autonomous research agent for Claude Code** — adapted from [GPT-Researcher](https://github.com/assafelovic/gpt-researcher).

## Why This Exists

GPT-Researcher is an excellent open-source project that builds an autonomous research agent using LLMs. It simulates an orchestrator by:
- Using LLMs to select agents
- Using LLMs to generate sub-queries
- Using LLMs to coordinate research
- Implementing async parallelism with custom code
- Managing multiple retrievers (Tavily, Google, Bing, etc.)

**But Claude Code already IS the orchestrator.**

Claude Code natively provides:
- **WebSearch** — replaces all search retrievers
- **WebFetch** — replaces web scraping modules
- **MCP servers** — extensible data sources (Context7 for docs, claude-mem for memory, etc.)
- **Task agents** — native parallel execution with `run_in_background`
- **Quality gates** — built into command workflows
- **Read/Glob/Grep** — local document research

Instead of building infrastructure to simulate orchestration, we use Claude Code's native capabilities directly.

## Installation

Copy the commands and skills to your Claude Code config:

```bash
# Commands
cp commands/_lookup.md ~/.claude/commands/
cp commands/_research.md ~/.claude/commands/
cp commands/_deep-research.md ~/.claude/commands/
cp commands/_research-team.md ~/.claude/commands/

# Skills
cp skills/researcher.md ~/.claude/skills/
```

## Commands

| Command | Description | Use Case |
|---------|-------------|----------|
| `/lookup` | Quick single-pass research | Simple factual questions |
| `/research` | Standard research with report | General research tasks |
| `/deep-research` | Multi-level exhaustive research | Complex topics requiring depth |
| `/research-team` | Multi-agent with review cycles | High-quality, verified reports |

## Architecture Comparison

### GPT-Researcher Architecture
```
User Query
    ↓
[LLM Agent Selection] ← LLM call
    ↓
[LLM Sub-query Generation] ← LLM call
    ↓
[Retriever Selection] ← Config + LLM
    ↓
[Async Parallel Research] ← Custom asyncio code
    ↓
[LLM Summarization] ← LLM call
    ↓
[LLM Report Generation] ← LLM call
    ↓
Report
```

### Claude Researcher Architecture
```
User Query
    ↓
/research command
    ↓
Claude Code (IS the orchestrator)
    ├── WebSearch (parallel queries)
    ├── WebFetch (parallel fetching)
    ├── Task agents (parallel research)
    └── Native synthesis
    ↓
Report
```

**Key difference:** No LLM calls just for orchestration. Claude Code handles planning, execution, and synthesis natively.

## Command Details

### `/lookup` — Quick Research

Fast, single-pass research for simple questions.

```
/lookup "What is the current price of Bitcoin?"
```

**Flow:**
```
WebSearch (2-3 parallel queries)
    ↓
WebFetch (top 2-3 URLs)
    ↓
Concise answer with sources
```

### `/research` — Standard Research

Comprehensive research with structured report.

```
/research "Impact of AI on software development" --depth=deep --format=report
```

**Options:**
- `--depth=shallow|medium|deep` (default: medium)
- `--sources=N` (default: 10)
- `--format=report|outline|resources` (default: report)
- `--tone=objective|analytical|explanatory` (default: objective)
- `--lang=XX` (default: en)

**Flow:**
```
Query Analysis
    ↓
Sub-query Generation (3-7 based on depth)
    ↓
Parallel WebSearch + WebFetch
    ↓
Content Synthesis
    ↓
Report Generation with Citations
```

### `/deep-research` — Exhaustive Research

Multi-level recursive research for complex topics.

```
/deep-research "The future of autonomous AI agents" --breadth=5 --depth=3
```

**Options:**
- `--breadth=N` (default: 5, parallel branches)
- `--depth=N` (default: 3, recursion levels)
- `--max-sources=N` (default: 30)
- `--focus=comprehensive|technical|business|academic`

**Flow:**
```
Query
  ├── Branch 1
  │     ├── Sub-query 1.1 → sources
  │     │     ├── Sub-sub 1.1.1 → sources
  │     │     └── Sub-sub 1.1.2 → sources
  │     └── Sub-query 1.2 → sources
  ├── Branch 2
  │     └── ...
  └── Branch N
        └── ...
              ↓
Comprehensive Report with Full Coverage
```

### `/research-team` — Multi-Agent Research

Collaborative research with quality gates and review cycles.

```
/research-team "Comparison of cloud databases 2024" --agents=4 --quality=high
```

**Options:**
- `--agents=N` (default: 3)
- `--review-cycles=N` (default: 2)
- `--min-sources=N` (default: 20)
- `--quality=standard|high|academic`

**Agent Roles:**
| Agent | Role |
|-------|------|
| Chief Editor | Plans outline, coordinates workflow |
| Research Agents | Parallel research on subtopics |
| Reviewer | Quality check, identifies gaps |
| Reviser | Incorporates feedback |
| Publisher | Final formatting |

**Flow:**
```
Chief Editor (Claude Code)
      │
      ├── [PARALLEL] Research Agent 1 → findings
      ├── [PARALLEL] Research Agent 2 → findings
      └── [PARALLEL] Research Agent 3 → findings
              ↓
         Draft Report
              ↓
         Reviewer Agent → feedback
              ↓
         Reviser Agent → improved draft
              ↓ (repeat until quality gate passes)
         Publisher Agent → Final Report
```

**Quality Thresholds:**
| Level | Min Sources | Min Words | Review Score |
|-------|-------------|-----------|--------------|
| standard | 15 | 2000 | 7/10 |
| high | 25 | 4000 | 8/10 |
| academic | 40 | 6000 | 9/10 |

## Tool Mapping

| GPT-Researcher | Claude Researcher |
|----------------|-------------------|
| Tavily, Google, Bing, DuckDuckGo | `WebSearch` |
| Web scraping (BeautifulSoup, Playwright) | `WebFetch` + Jina Reader fallback |
| ArXiv, Semantic Scholar, PubMed | `WebSearch` with site filters |
| MCP integration | Native MCP servers |
| Vector store / Memory | `claude-mem` MCP |
| Local documents | `Read`, `Glob`, `Grep` |
| asyncio.gather() | Task agents with `run_in_background` |
| LangChain/LangGraph | Not needed — Claude Code is the orchestrator |

## When to Use What

| Situation | Command |
|-----------|---------|
| "What is X?" | `/lookup` |
| "Research X and write a report" | `/research` |
| "I need to understand everything about X" | `/deep-research` |
| "I need a high-quality, verified report on X" | `/research-team` |

## Extending

### Add Custom MCP Sources

Claude Researcher works with any MCP server. Add specialized data sources:

```json
// In your MCP config
{
  "servers": {
    "arxiv": { "command": "mcp-arxiv" },
    "github": { "command": "mcp-github" }
  }
}
```

### Customize Prompts

Edit `skills/researcher.md` to modify:
- Sub-query generation prompts
- Report structure templates
- Source curation criteria
- Quality thresholds

## Credits

- Original concept: [GPT-Researcher](https://github.com/assafelovic/gpt-researcher) by Assaf Elovic
- Adapted for Claude Code by leveraging native orchestration capabilities

## License

MIT — same as the original GPT-Researcher.
