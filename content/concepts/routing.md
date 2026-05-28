---
title: Routing
created: 2026-05-28
updated: '2026-05-28'
type: concept
tags:
- routing
- orchestrator-workers
- tool-use
sources: [https://langchain-ai.github.io/langgraph/, https://openai.github.io/openai-agents-python/]
confidence: high
---

# Routing

## Definition

**Routing** is the pattern where an agent or orchestrator decides which specialized
path, tool, or sub-agent should handle a given request. Rather than a single monolithic
agent attempting everything, routing delegates to the most appropriate handler based
on the task type, domain, or input characteristics.

Routing is a core component of multi-agent and orchestrator-worker architectures. It
sits between the initial request intake and the actual execution, acting as a
decision layer that directs work to the right capability.

## How It Works

The routing pattern typically involves:

1. **Input analysis** — The router examines the user request, extracts intent,
   domain, or task type
2. **Decision** — Based on the analysis, the router selects a handler (a specific
   tool, agent, or processing path)
3. **Delegation** — The request is forwarded to the chosen handler with relevant
   context
4. **Result aggregation** — The router (or orchestrator) collects the handler's
   output and returns it to the user

### Implementation Approaches

- **LLM-based routing** — An LLM evaluates the input and selects the best handler.
  Flexible but adds latency and cost.
- **Rule-based routing** — Keyword matching, intent classification, or regex patterns
  direct to handlers. Fast and deterministic but less adaptable.
- **Hybrid routing** — Rule-based for common cases, LLM-based fallback for ambiguous
  or novel inputs.

## When to Use

**Best suited for:**
- Multi-agent systems where different agents specialize in different domains
- Tool-heavy applications where not all tools are relevant to every request
- Complex workflows with distinct phases that require different capabilities
- Systems that need to balance cost by routing simple queries to cheaper handlers

**Trade-offs:**
- **Latency** — Routing adds an extra decision step before execution
- **Routing errors** — Misclassification sends requests to the wrong handler,
  producing poor results
- **Maintenance** — Adding new handlers requires updating the routing logic
- **Overhead** — For simple applications, routing may be unnecessary complexity

## Relationship to Other Patterns

- [[planning-pattern]] — Planning determines the sequence of steps within a chosen
  path; routing determines which path to take first
- [[tool-use-pattern]] — Tool use is the mechanism the chosen handler employs;
  routing decides which tool or agent handles the request
- [[react-pattern]] — ReAct can implement routing as one of its action steps,
  but routing is typically a pre-processing decision before the main loop
- orchestrator-workers — The orchestrator often includes a routing layer to
  distribute tasks among workers

## Framework Support

- [[langgraph]] supports routing through conditional edges in the graph that
  route based on node output state
- [[openai-agents-sdk]] implements routing through its handoff mechanism where
  one agent delegates to another based on capability
- [[crewai]] supports routing through task assignment where the manager routes
  subtasks to appropriate worker agents
- [[langchain]] offers routing through its RouterChain abstraction and conditional
  branching in LCEL pipelines

## Related Concepts

[[planning-pattern]], [[tool-use-pattern]], [[react-pattern]], orchestrator-workers

## Sources

- LangGraph conditional routing documentation
- OpenAI Agents SDK handoff mechanism
- CrewAI task assignment patterns
