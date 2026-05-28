---
title: Google Agent Development Kit (ADK)
created: 2026-05-28
updated: '2026-05-28'
type: entity
tags:
- google
- open-source
- agent-framework
sources: [https://github.com/google/adk]
confidence: high
---

# Google Agent Development Kit (ADK)

## Definition

The **Google Agent Development Kit (ADK)** is an open-source framework by Google for
building, testing, and deploying LLM-powered agents. It is designed to integrate
seamlessly with Google's ecosystem — Gemini models, Vertex AI, Google Cloud tools,
and Google Workspace APIs — while remaining flexible enough for custom model routing
and external tool integration.

ADK positions itself as a lightweight, opinionated alternative to heavier frameworks
like [[langchain]] and [[langgraph]], emphasizing simplicity, built-in observability,
and production-ready deployment patterns.

## Architecture

ADK follows a modular agent architecture:

1. **Agent definition** — Agents are defined as Python classes or functions with
   explicit tool declarations, system prompts, and model configurations
2. **Tool registry** — Tools are registered as callable functions with structured
   schemas (JSON Schema compatible), similar to the [[tool-use-pattern]]
3. **Execution loop** — The framework handles the generate → tool-call → observe
   loop automatically, reducing boilerplate compared to manual ReAct implementations
4. **Multi-agent routing** — Supports sub-agent delegation and routing between
   specialized agents, enabling [[orchestrator-workers]] patterns

## Key Features

- **Gemini-native** — First-class support for Gemini Pro, Gemini Flash, and
  upcoming Gemini variants with automatic function calling
- **Built-in tracing** — Every agent step is logged and traceable, enabling
  debugging and performance analysis without external tooling
- **Vertex AI integration** — Direct deployment to Vertex AI for scaling and
  production serving
- **Tool sandboxing** — Tools can be executed in isolated environments for safety
- **Multi-model routing** — Route different agent steps to different models based
  on capability requirements (e.g., fast model for routing, strong model for reasoning)

## When to Use

**Best suited for:**
- Teams already in the Google Cloud ecosystem
- Applications requiring Gemini model capabilities
- Production deployments needing built-in observability
- Multi-agent systems with routing and delegation requirements

**Trade-offs:**
- **Google-centric** — Less flexible for non-Google model providers compared to
  [[langchain]] or [[openai-agents-sdk]]
- **Newer ecosystem** — Smaller community and fewer third-party integrations than
  established frameworks
- **Opinionated** — Less flexibility in execution model compared to [[langgraph]]

## Relationship to Other Frameworks

- [[langchain]] — More mature ecosystem, broader model support, but heavier and
  more complex
- [[langgraph]] — More flexible graph-based orchestration, but requires more
  boilerplate for basic agent loops
- [[openai-agents-sdk]] — Simpler for OpenAI-specific use cases, but narrower
  model support
- [[crewai]] — More opinionated on role-based multi-agent patterns, but less
  flexible for custom architectures

## Related Concepts

[[tool-use-pattern]], [[orchestrator-workers]], [[routing]], [[function-calling]],
[[react-pattern]]

## Sources

- Google ADK GitHub repository
- Google Cloud Vertex AI documentation
