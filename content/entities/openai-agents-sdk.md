---
title: OpenAI Agents SDK
created: 2026-05-28
updated: '2026-05-28'
type: entity
tags:
- openai-agents
- tool-use
- openai
- open-source
sources: [https://openai.github.io/openai-agents-python/, https://github.com/openai/openai-agents-python]
confidence: high
---

# OpenAI Agents SDK

## Overview

The OpenAI Agents SDK is an official Python framework released by OpenAI for building agentic AI applications. It provides a lightweight, composable interface for constructing agents that can reason, use tools, and hand off tasks to other agents. The SDK was designed to sit between raw model APIs (like the OpenAI Chat Completions API) and heavier orchestration frameworks like [[langchain]] or [[crewai]].

It is an open-source project under the MIT license, meaning anyone can use, modify, and distribute it freely. This positions it as a baseline reference implementation for what OpenAI considers a well-designed agent architecture.

The SDK reflects OpenAI's view that agent building should be straightforward: define an agent with instructions and tools, run it, and get back a traceable execution. It deliberately avoids complex abstractions in favor of composability and clarity.

## Key Facts

| Attribute | Details |
|-----------|---------|
| Developer | OpenAI |
| Language | Python (primary); TypeScript/JavaScript also available |
| License | MIT |
| First Release | Late 2024 (preview), GA in 2025 |
| Model Compatibility | OpenAI models primarily; can work with others via adapter |
| GitHub | https://github.com/openai/openai-agents-python |
| Documentation | https://openai.github.io/openai-agents-python/ |

## Architecture Highlights

### Agent Model

- **Agent as a first-class object**: An `Agent` encapsulates a system prompt (instructions), a set of tools, and a handoff strategy. This is the core abstraction — everything else builds on it.
- **Tool use**: Native support for defining Python functions as tools; the SDK handles schema generation, argument validation, and result parsing. This makes it trivial to expose any Python function as a tool callable by the agent.
- **Handoffs**: Agents can delegate to other agents via structured handoffs, enabling multi-agent workflows without a heavy orchestrator. A handoff transfers full context to the receiving agent, which continues the conversation from that point.

### Execution Model

- **Runs and Traces**: Each agent execution produces a `Run` object with full traceability. You can inspect every tool call, every model response, and the complete reasoning trace. This is critical for debugging and [[evaluation]].
- **Guardrails**: Built-in input/output validation for safety-critical applications. Guardrails can reject or modify inputs before they reach the model and validate outputs before they are returned to the user.
- **Streaming**: Native support for streaming responses and tool-call events, enabling real-time user interfaces and progressive rendering.

### Design Philosophy

- **Minimal abstraction**: The SDK intentionally avoids complex DAG-based orchestration, favoring simple function composition. This contrasts with frameworks that model agent workflows as graphs or pipelines.
- **OpenAI-native**: Tightly integrated with OpenAI's model capabilities including structured outputs, function calling, and reasoning models. The SDK gets new features quickly when OpenAI releases them.
- **Extensible**: Can be combined with [[llamaindex]] for RAG pipelines or [[langgraph]] for complex state machines. The SDK does not try to be everything — it provides a solid foundation that other frameworks can build on.

## Framework Support

The SDK is itself a framework, but it interoperates with others:

- [[langchain]]: The SDK can be used within a LangChain tool-calling chain, or LangChain tools can be adapted to the SDK's tool interface. LangChain's broader ecosystem (retrieval, memory, callbacks) complements the SDK's lightweight core.
- [[langgraph]]: The SDK's agent model can serve as a node within a LangGraph state machine. This combines the SDK's clean tool-use interface with LangGraph's checkpointing, branching, and human-in-the-loop capabilities.
- [[crewai]]: The SDK and CrewAI serve different purposes — the SDK provides single-agent and handoff-based multi-agent, while CrewAI provides role-based delegation. They can be combined by using SDK agents as crew members.
- [[llamaindex]]: LlamaIndex query engines can be exposed as tools to SDK agents, creating knowledge-grounded agents with the SDK's execution model and LlamaIndex's retrieval capabilities.
- [[autogen]]: SDK agents can participate in AutoGen conversations through adapter patterns, combining the SDK's tool-use with AutoGen's multi-agent dialogue.
- [[google-adk]]: Both frameworks share a similar lightweight philosophy. Google ADK can serve as an alternative for teams committed to the Google ecosystem.
- **OpenAI API**: The SDK sits directly on top of the OpenAI API, providing a higher-level abstraction that handles tool schema management, run lifecycle, and trace generation.

## When to Use

The OpenAI Agents SDK is a strong choice when:

- **You want a lightweight, official framework** without the learning curve of heavier orchestration systems. The SDK is intentionally simple and well-documented.
- **You are already using OpenAI models** and want the tightest integration with their tool-calling and structured output capabilities.
- **You need handoff-based multi-agent workflows** without the complexity of a full orchestrator. The handoff mechanism is clean and context-preserving.
- **You want built-in tracing and observability** out of the box. The SDK's Run objects provide complete execution traces without additional instrumentation.
- **You need guardrails for production safety**. The SDK's input/output validation is designed for safety-critical applications.
- **Your team values minimal abstraction** and prefers to compose simple primitives rather than configure complex orchestration pipelines.

When to consider alternatives:

- **You need complex graph-based orchestration** with loops, branching, and checkpointing. In this case, [[langgraph]] is a better fit.
- **You want role-based multi-agent delegation** with structured task assignment. [[crewai]] provides this natively.
- **You need framework-agnostic model support**. The SDK is optimized for OpenAI models; using other models requires adapters.
- **You need a TypeScript/JavaScript-first solution**. While a JS/TS version exists, the Python SDK is the primary and most mature implementation.
- **You need heavy RAG integration**. While the SDK can use LlamaIndex tools, [[llamaindex]] or [[langchain]] provide more comprehensive retrieval ecosystems.

## Relationships

- Contrasts with [[langchain]] (heavier, more abstracted, broader ecosystem) and [[crewai]] (role-based multi-agent focus with opinionated delegation patterns)
- Complements [[llamaindex]] for knowledge-grounded agents where retrieval is a primary capability
- Can serve as the execution layer within a [[langgraph]] state machine, combining lightweight tool-use with graph-based control flow
- Represents OpenAI's official answer to third-party agent frameworks, signaling the company's view on what a good agent architecture looks like
- Contrasts with [[anthropic]]'s approach: Anthropic focuses on protocol standardization ([[mcp]]) and product surfaces (Claude Code), while OpenAI focuses on SDK-based developer tooling

## Related Concepts

[[function-calling]], [[langgraph]], [[llamaindex]], [[coding-agents]], [[langchain]], [[crewai]], [[tool-use-pattern]], [[evaluation]], [[orchestrator-workers]], [[routing]]

## Sources

- OpenAI Agents SDK documentation: https://openai.github.io/openai-agents-python/
- OpenAI Agents Python repository: https://github.com/openai/openai-agents-python
