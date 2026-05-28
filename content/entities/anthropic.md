---
title: Anthropic
created: 2026-07-11
updated: '2026-05-28'
type: entity
tags:
- anthropic
- open-source
sources: [https://www.anthropic.com/, https://docs.anthropic.com/, https://modelcontextprotocol.io/introduction]
confidence: high
---

# Anthropic

## Overview

Anthropic is an AI research company founded in 2021 by former OpenAI researchers. It develops the Claude model family and has become one of the central platform companies in the AI agent ecosystem, especially for tool-using and coding-oriented workloads.

Its strategic position in agents comes from three layers at once: frontier models, agent-facing product surfaces, and protocol influence. Claude models are widely used for long-context reasoning and tool orchestration, Claude Code is a direct coding-agent product, and Anthropic originated [[mcp]], which has become a major standardization effort for agent tool connectivity.

## Key Facts

- Founded in 2021 by Dario Amodei, Daniela Amodei, and other former OpenAI researchers
- Core model family: Claude (Haiku, Sonnet, Opus tiers)
- Major agent-facing surfaces: API, Claude Code, Computer Use, Artifacts
- Originator of [[mcp]] (Model Context Protocol)
- Positioned strongly in enterprise and developer agent workflows
- Headquarters: San Francisco, CA

## Products and Capabilities

### Claude model family

Anthropic's model lineup is organized into capability tiers:

- **Claude Haiku**: Fast, cost-efficient variant for high-volume orchestration, classification, and simple tool-calling loops. Ideal for [[routing]] decisions and high-throughput [[prompt-chaining]] steps.
- **Claude Sonnet**: Balanced variant for general-purpose agent work, coding tasks, and multi-step reasoning. The workhorse for most [[coding-agents]] implementations.
- **Claude Opus**: Strongest reasoning variant for complex planning, deep code analysis, and tasks requiring extended chain-of-thought. Suitable for [[planning-pattern]] and [[orchestrator-workers]] architectures.

This tiered approach matters for agents because orchestration workloads often care about the total loop cost across many steps, not just single-turn quality. A well-designed agent system routes simple decisions to Haiku, reserves Sonnet for standard tasks, and invokes Opus only when necessary.

### Claude Code

Claude Code is a terminal-based coding agent product that brings Anthropic models directly into developer workflows. It features:

- Direct filesystem access with read/write/edit capabilities
- Code execution with sandboxed environments
- Git integration for understanding repository context
- Multi-file editing with coherent reasoning across the codebase
- Natural language task specification

It matters because coding agents are one of the highest-value and most infrastructure-intensive categories in the broader AI agent market, and Claude Code competes directly with products like Devin, Codex, and open-source alternatives.

### Computer Use and tool interaction

Anthropic has pushed browser and desktop interaction forward through Computer Use-style workflows. This extends agents beyond API tools into GUI-bound environments where reasoning must stay coupled to perception and action. Computer Use enables agents to:

- Navigate web interfaces visually
- Interact with desktop applications
- Handle tasks that lack programmatic APIs
- Demonstrate the full spectrum of [[tool-use-pattern]] beyond structured function calling

### Model Context Protocol (MCP)

Anthropic introduced the Model Context Protocol as an open standard for connecting AI models to external tools, data sources, and services. MCP matters because it solves the integration fragmentation problem: instead of every framework implementing its own tool-calling convention, MCP provides a unified interface that any MCP-compatible host can use with any MCP-compatible server.

This gives Anthropic protocol-level influence across the entire agent ecosystem, not just its own products.

## Framework Support

Anthropic's capabilities are accessible through multiple frameworks:

- [[langchain]]: LangChain provides a `ChatAnthropic` model wrapper that supports Claude's tool-use API, structured outputs, and streaming.
- [[langgraph]]: Claude models can serve as the reasoning node in LangGraph state machines, with full checkpointing and human-in-the-loop support.
- [[crewai]]: Claude models can be used as the LLM backend for CrewAI agents, benefiting from the role-based multi-agent architecture.
- [[openai-agents-sdk]]: While the SDK is OpenAI-focused, adapter patterns allow Claude models to be used as the reasoning engine.
- [[autogen]]: Claude can serve as an agent backend in AutoGen's conversational multi-agent system.
- [[llamaindex]]: Claude integrates with LlamaIndex's query engines for RAG pipelines.
- **Native API**: Anthropic's own API provides direct access to Claude models with tool-use support, extended context windows, and structured output formats.

## When to Use Anthropic Products

Claude models and Anthropic products are a strong choice when:

- **Long context is critical**. Claude's context window is among the largest in the industry, making it suitable for analyzing large codebases, long documents, or extended conversation histories.
- **Tool-use quality matters**. Claude's function-calling accuracy and structured output reliability are competitive benchmarks for agent systems.
- **You need a coding agent**. Claude Code provides a production-ready coding agent with minimal setup.
- **Protocol standardization is valuable**. Using MCP-compatible tools gives you framework-agnostic tool integration.
- **Safety and alignment are priorities**. Anthropic's focus on constitutional AI and safety research may be relevant for regulated or safety-critical applications.

When to consider alternatives:

- **Cost-sensitive high-volume workloads** where cheaper open-source models may suffice.
- **Open-source-only environments** where cloud API access is not permitted.
- **Tasks requiring specialized capabilities** not offered by Claude (e.g., image generation, audio processing).

## Strategic Importance

Anthropic matters in the AI agents landscape because it competes on more than raw model quality. Its influence comes from how well its models fit real agent loops: long context, iterative tool use, coding tasks, and structured multi-step execution.

In market terms, Anthropic is also a direct demand driver for inference-heavy workloads. Agent products consume meaningfully more tokens and more tool-roundtrips than chat-only products, so a company that wins coding agents and enterprise tool-using workflows can have outsized infrastructure impact.

Anthropic's dual strategy — building both frontier models (Claude) and protocol infrastructure (MCP) — positions it to benefit regardless of which agent framework becomes dominant.

## Relationships

- [[mcp]] — Anthropic introduced the protocol and continues to steward its development
- [[coding-agents]] — Anthropic is a major vendor through Claude Code and the Claude API
- [[function-calling]] — Claude models compete heavily on tool-use quality and structured output reliability
- [[openai-agents-sdk]] — contrast between Anthropic's protocol/product approach and OpenAI's SDK-centric approach
- [[tool-use-pattern]] — Claude's Computer Use pushes the boundaries of what tools agents can interact with

## Related Concepts

[[mcp]], [[coding-agents]], [[function-calling]], [[openai-agents-sdk]], [[tool-use-pattern]], [[langchain]], [[langgraph]]

## Sources

- Anthropic company and product documentation
- MCP specification and launch materials
- Claude API documentation
