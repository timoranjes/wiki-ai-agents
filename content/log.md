# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-05-28] create | Wiki initialized
- Domain: AI Agents knowledge base
- Structure created with SCHEMA.md, index.md, log.md
- Directories: raw/{articles,papers,github,sessions}, entities, concepts, comparisons, queries, _archive

## [2026-05-28] create | Entity page: openai-agents-sdk.md
- OpenAI Agents SDK entity page
- Covers overview, architecture, execution model, and design philosophy
- Linked to: [[langchain]], [[crewai]], [[llamaindex]], [[langgraph]], [[multi-agent]]

## [2026-05-28] create | Entity page: llamaindex.md
- LlamaIndex entity page
- Covers data ingestion, query engine, agentic capabilities, and indexing
- Linked to: [[openai-agents-sdk]], [[langchain]], [[crewai]], [[rag]], [[orchestrator-workers]]

## [2026-05-28] create | Comparison page: langchain-vs-crewai.md
- LangChain vs CrewAI comparison
- Compares design philosophy, multi-agent capabilities, ecosystem, and use-case fit
- Verdict: complementary rather than competing; combine both for production systems
- Linked to: [[langchain]], [[crewai]], [[openai-agents-sdk]], [[langgraph]]

## [2026-05-28] create | Comparison page: single-vs-multi-agent.md
- Single-Agent vs Multi-Agent architectures comparison
- Covers complexity, cost, latency, scalability, reliability, and framework choices
- Verdict: spectrum not binary; start single-agent, evolve as requirements grow
- Linked to: [[orchestrator-workers]], [[hierarchical]], [[swarm]], [[openai-agents-sdk]], [[crewai]], [[autogen]], [[langgraph]]

## [2026-05-28] create | Concept page: react-pattern.md
- ReAct (Reasoning + Acting) pattern
- Covers definition, Thought/Action/Observation loop, use cases, and trade-offs
- Linked to: [[tool-use-pattern]], [[planning-pattern]], [[reflection-pattern]], [[prompt-chaining]], [[langchain]], [[openai-agents-sdk]], [[crewai]], [[langgraph]]

## [2026-05-28] create | Concept page: reflection-pattern.md
- Reflection/self-correction pattern
- Covers generation → critique → revise cycle, variants (single, iterative, dual-LLM), and trade-offs
- Linked to: [[react-pattern]], [[planning-pattern]], [[evaluation]], [[tool-use-pattern]], [[prompt-chaining]], [[langchain]], [[langgraph]], [[openai-agents-sdk]], [[crewai]]

## [2026-05-28] create | Concept page: planning-pattern.md
- Planning/subgoal decomposition pattern
- Covers plan generation (linear, DAG, hierarchical), execution, replanning, and strategy comparison table
- Linked to: [[react-pattern]], [[reflection-pattern]], [[tool-use-pattern]], [[routing]], [[langgraph]], [[langchain]], [[crewai]], [[openai-agents-sdk]], [[autogen]]

## [2026-05-28] create | Concept page: tool-use-pattern.md
- Tool use/function calling pattern
- Covers tool definition, selection, execution, result integration, and design best practices
- Linked to: [[react-pattern]], [[planning-pattern]], [[reflection-pattern]], [[routing]], [[langchain]], [[openai-agents-sdk]], [[langgraph]], [[crewai]], [[autogen]], [[google-adk]]

## [2026-05-28] ingest | AI agents daily pipeline
- Raw files processed:
  - raw/papers/unknown-setupx-can-llm-agents-learn-from-past-failures-in-functionality-correct-code-rep.md
  - raw/papers/unknown-persona2web-benchmarking-personalized-web-agents-for-contextual-reasoning-with-u.md
- Pages created:
  - concepts/setupx.md
  - concepts/persona2web.md
- Pages updated:
  - index.md
- Pages skipped:
  - none

## [2026-05-28] update | AI agents wiki schema correction
- Rebuilt pages that incorrectly used ministry-wiki section headers and content structure
- Updated files:
  - entities/anthropic.md
  - entities/mcp.md
  - concepts/react-arch.md
  - concepts/coding-agents.md
  - concepts/function-calling.md
  - concepts/swe-bench.md
  - SCHEMA.md
- Removed invalid AI-agents page structure patterns such as `關鍵經文`, `職事啟示`, and `實行應用` from the corrected files
- Remaining follow-up: resolve vault-wide naming drift, invalid tags, and broken wikilinks beyond this schema correction pass

## [2026-05-28] update | Vault normalization and cron hardening
- Audited and normalized wiki frontmatter, tags, wikilinks, canonical slugs, and index coverage
- Pages rewritten or normalized:
- Updated files:
  - index.md
  - log.md
  - cron job 8e838fb769b3 prompt

## [2026-05-28] update | Vault normalization and cron hardening
- Audited entire AI-agents vault for canonical slugs, frontmatter, tags, wikilinks, and index coverage
- Renamed files:
  - concepts/react-arch.md -> concepts/react-pattern.md
- Normalized pages:
  - comparisons/langchain-vs-crewai.md
  - comparisons/single-vs-multi-agent.md
  - concepts/coding-agents.md
  - concepts/function-calling.md
  - concepts/persona2web.md
  - concepts/planning-pattern.md
  - concepts/react-pattern.md
  - concepts/reflection-pattern.md
  - concepts/setupx.md
  - concepts/swe-bench.md
  - concepts/tool-use-pattern.md
  - entities/anthropic.md
  - entities/autogen.md
  - entities/crewai.md
  - entities/langchain.md
  - entities/langgraph.md
  - entities/llamaindex.md
  - entities/mcp.md
  - entities/openai-agents-sdk.md
- Rebuilt index.md from actual filesystem state
- Hardened Stage 2 cron prompt to enforce SCHEMA-first processing and anti-corruption checks

## [2026-05-28] update | page quality pass
- Strengthened explicit `## Related Concepts` sections on framework, benchmark, and comparison pages that previously relied only on incidental inline links.
- Replaced empty `sources: []` frontmatter on 8 pages with concrete documentation sources and added source sections to 10 pages.
- Tightened page tags for navigability and taxonomy quality on frameworks, concepts, and comparison pages.
- Updated files: `entities/autogen.md`, `entities/crewai.md`, `entities/langchain.md`, `entities/langgraph.md`, `entities/llamaindex.md`, `entities/openai-agents-sdk.md`, `concepts/persona2web.md`, `concepts/setupx.md`, `comparisons/langchain-vs-crewai.md`, `comparisons/single-vs-multi-agent.md`

## [2026-05-28] update | page quality repair
- Repaired 10 pages that were accidentally truncated during the first quality-pass patch attempt.
- Restored full page bodies while preserving stronger tags, explicit related-concepts sections, and source metadata.
- Repaired files: `entities/autogen.md`, `entities/crewai.md`, `entities/langchain.md`, `entities/langgraph.md`, `entities/llamaindex.md`, `entities/openai-agents-sdk.md`, `concepts/persona2web.md`, `concepts/setupx.md`, `comparisons/langchain-vs-crewai.md`, `comparisons/single-vs-multi-agent.md`

## [2026-05-28] update | sources completion pass
- Filled the remaining 9 empty `sources: []` frontmatter blocks so all 19 live pages now have explicit source metadata.
- Tightened weak inline concept references by converting plain-text taxonomy references into wikilinks where appropriate.
- Updated files: `entities/anthropic.md`, `entities/mcp.md`, `concepts/react-pattern.md`, `concepts/planning-pattern.md`, `concepts/coding-agents.md`, `concepts/function-calling.md`, `concepts/swe-bench.md`, `concepts/tool-use-pattern.md`, `concepts/reflection-pattern.md`

## [2026-05-28] lint | link integrity pass
- Scanned all 19 pages for unresolved wikilinks; found 4 broken targets.
- Created `concepts/routing.md` (2 refs from planning-pattern and tool-use-pattern — warranted a full concept page).
- Stripped 4 aspirational single-ref wikilinks to plain text: `google-adk`, `evaluation`, `prompt-chaining`, `orchestrator-workers`.
- Updated `index.md` to include routing entry and bumped total to 20 pages.
- Verified zero remaining broken wikilinks across the vault (20 pages: 8 entities, 10 concepts, 2 comparisons).

## [2026-05-28] update | bulk deepening + aspirational page creation
- Created 4 new concept pages: `concepts/evaluation.md`, `concepts/prompt-chaining.md`, `concepts/orchestrator-workers.md`, `entities/google-adk.md`
- Deepened 10 thinnest pages via parallel subagents (5 per subagent):
  - setupx: 47→185 lines, persona2web: 49→196 lines, react-pattern: 62→257 lines
  - anthropic: 63→121 lines, openai-agents-sdk: 63→103 lines
  - mcp: 63→108 lines, swe-bench: 64→151 lines, coding-agents: 66→151 lines
  - function-calling: 66→152 lines, llamaindex: 67→116 lines
- All deepened pages gained `## Framework Support`, `## When to Use`, and expanded `## Related Concepts` (5+ valid wikilinks each)
- Vault now: 24 pages, 0 broken wikilinks, average 128 lines, zero pages under 70 lines
- Updated `index.md` to 24 entries (9 entities, 13 concepts, 2 comparisons)
