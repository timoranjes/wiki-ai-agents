# Wiki Schema

## Domain

AI Agents knowledge base — covering agent architectures, frameworks, patterns, platforms, evaluation methods, product categories, protocols, and emerging research.

Focus areas:
- **Agent Architectures** — single-agent, multi-agent, swarm, hierarchical, orchestrator-workers
- **Frameworks & SDKs** — LangChain, LangGraph, CrewAI, AutoGen, OpenAI Agents SDK, Google ADK, LlamaIndex, Semantic Kernel
- **Agentic Patterns** — ReAct, Reflection, Planning, Tool Use, Memory, RAG, Code Generation, Evaluation
- **Platforms & Providers** — OpenAI, Anthropic, Google, Meta, open-source model providers
- **Evaluation & Benchmarking** — agent benchmarks, task completion metrics, safety evaluation
- **Research** — academic papers, industry research, survey reports

## Conventions

- File names: lowercase, hyphens, no spaces (e.g., `react-pattern.md`, `openai-agents-sdk.md`)
- Every wiki page starts with YAML frontmatter (see below)
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page)
- When updating a page, always bump the `updated` date
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`
- Source attribution lives in `sources:` frontmatter only; do not use inline provenance footnotes
- This is an AI agents wiki, not a ministry wiki. Do not use ministry-specific section headers such as `關鍵經文`, `職事啟示`, `實行應用`, or `來源` as a content model for agent pages.

## Frontmatter

```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-file.md]
confidence: high | medium | low
contested: true
contradictions: [other-page-slug]
---
```

## Recommended Page Structures

### Entity pages

Use sections like:
- `## Overview`
- `## Key Facts`
- `## Products and Capabilities` or `## Architecture`
- `## Strategic Importance` or `## Ecosystem Role`
- `## Relationships`
- `## Related Concepts`
- `## Sources`

### Concept pages

Use sections like:
- `## Definition`
- `## Mechanics` or `## Core Components`
- `## Why It Matters`
- `## Trade-offs` or `## Limitations`
- `## Ecosystem Role` or `## Evaluation Considerations`
- `## Related Concepts`
- `## Sources`

### Comparison pages

Use sections like:
- `## Scope`
- `## Comparison Table`
- `## Key Differences`
- `## Trade-offs`
- `## Verdict`
- `## Related Concepts`
- `## Sources`

## raw/ Frontmatter

Raw sources get a small frontmatter block so re-ingests can detect drift:

```yaml
---
source_url: https://example.com/article
ingested: YYYY-MM-DD
sha256: <hex digest of the raw content below the closing --->
---
```

## Tag Taxonomy

**Architecture**: `single-agent`, `multi-agent`, `swarm`, `hierarchical`, `orchestrator-workers`
**Pattern**: `react`, `reflection`, `planning`, `tool-use`, `memory`, `rag`, `code-generation`, `evaluation`, `prompt-chaining`, `routing`
**Framework**: `langchain`, `langgraph`, `crewai`, `autogen`, `openai-agents`, `google-adk`, `llamaindex`, `semantic-kernel`
**Platform**: `openai`, `anthropic`, `google`, `meta`, `open-source`
**Evaluation**: `benchmark`, `metric`, `safety`, `leaderboard`
**Meta**: `comparison`, `survey`, `tutorial`, `research-paper`

Rule: every tag on a page must appear in this taxonomy. If a new tag is needed, add it here first, then use it.

## Page Thresholds

- **Create a page** when an entity/concept appears in 2+ sources OR is central to one source
- **Add to existing page** when a source mentions something already covered
- **DON'T create a page** for passing mentions, minor details, or things outside the domain
- **Split a page** when it exceeds ~200 lines — break into sub-topics with cross-links
- **Archive a page** when its content is fully superseded — move to `_archive/`, remove from index

## Entity Pages

One page per notable entity (framework, platform, company, product, protocol, or person). Include overview, distinguishing facts, ecosystem relationships, and source-backed significance.

## Concept Pages

One page per concept or pattern. Include definition, operating mechanics, why it matters, trade-offs or limits, and relationships to adjacent concepts.

## Comparison Pages

Side-by-side analyses. Include what is being compared, the evaluation dimensions, the trade-offs, and a verdict or synthesis.

## Update Policy

When new information conflicts with existing content:
1. Check the dates — newer sources generally supersede older ones
2. If genuinely contradictory, note both positions with dates and sources
3. Mark the contradiction in frontmatter: `contradictions: [page-name]`
4. Flag for user review in the lint report
