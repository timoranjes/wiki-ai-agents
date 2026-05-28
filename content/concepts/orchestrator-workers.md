---
title: Orchestrator-Workers
created: 2026-05-28
updated: '2026-05-28'
type: concept
tags:
- multi-agent
- orchestrator-workers
- architecture
sources: [https://openai.github.io/openai-agents-python/, https://langchain-ai.github.io/langgraph/]
confidence: high
---

# Orchestrator-Workers

## Definition

**Orchestrator-Workers** is a multi-agent architecture pattern where a central
orchestrator agent decomposes tasks, delegates subtasks to specialized worker agents,
and aggregates their results. The orchestrator does not execute the work itself — it
manages the workflow, handles routing, error recovery, and result synthesis.

This pattern is one of the most common multi-agent architectures because it cleanly
separates coordination logic from execution logic, enabling specialization and
independent scaling of each component.

## Architecture

```
┌─────────────────┐
│   Orchestrator   │ ← Task decomposition, routing, aggregation
│   (Manager)      │
└────┬────┬────┬───┘
     │    │    │
     ▼    ▼    ▼
┌─────┐┌─────┐┌─────┐
│ W1  ││ W2  ││ W3  │  ← Specialized workers (coding, research, etc.)
└─────┘└─────┘└─────┘
```

### Orchestrator Responsibilities
- **Task decomposition** — Break the user's request into subtasks
- **Routing** — Assign each subtask to the appropriate worker based on capability
- **Dependency management** — Handle cases where one worker's output feeds another's input
- **Error recovery** — Retry failed subtasks, fallback to alternative workers
- **Aggregation** — Combine worker outputs into a coherent final response

### Worker Responsibilities
- **Execution** — Complete the assigned subtask using their specialized capabilities
- **Reporting** — Return structured results to the orchestrator
- **Error signaling** — Report failures clearly so the orchestrator can handle them

## Implementation Patterns

### Single-Model Orchestrator
The orchestrator and workers all use the same LLM but with different system prompts
and tool sets. Simple to implement but doesn't leverage model specialization.

### Multi-Model Orchestrator
Different workers use different models optimized for their domain (e.g., a coding
model for code workers, a reasoning model for analysis workers). The orchestrator
handles model routing.

### Hierarchical Orchestration
Workers can themselves be orchestrators for sub-subtasks, creating a tree structure.
Useful for deeply nested decompositions but adds complexity.

### Parallel vs Sequential
- **Parallel** — Workers execute simultaneously when subtasks are independent
- **Sequential** — Workers execute in order when subtasks have dependencies
- **Hybrid** — Parallel execution within dependency groups, sequential between groups

## When to Use

**Best suited for:**
- Complex tasks requiring multiple distinct capabilities
- Workflows where different subtasks benefit from different models or tool sets
- Systems that need to scale horizontally (add workers as capabilities grow)
- Applications requiring fault tolerance (worker failures don't crash the system)

**Trade-offs:**
- **Coordination overhead** — The orchestrator adds latency and token cost
- **Complexity** — More moving parts to debug, monitor, and maintain
- **Context management** — Passing relevant context between orchestrator and workers
  can consume significant tokens
- **Aggregation quality** — Combining multiple worker outputs coherently is non-trivial

## Framework Implementations

- [[openai-agents-sdk]] implements this through its handoff mechanism where the
  orchestrator agent delegates to specialized worker agents
- [[langgraph]] models it as a graph with a central manager node that routes to
  worker nodes with conditional edges
- [[crewai]] is explicitly built around this pattern — the Crew is the orchestrator,
  Agents are workers, and Tasks define the delegation structure
- [[autogen]] supports it through group chat orchestration where a manager agent
  coordinates with specialized worker agents

## Relationship to Other Patterns

- [[routing]] — The orchestrator includes a routing layer to decide which worker
  handles each subtask
- [[planning-pattern]] — Task decomposition is a form of planning; the orchestrator
  creates and manages the execution plan
- [[single-vs-multi-agent]] — Orchestrator-workers is the canonical multi-agent
  architecture; single-agent is the alternative when complexity doesn't warrant it
- [[coding-agents]] — Many coding agent systems use orchestrator-workers to separate
  code generation, testing, and review into different workers

## Related Concepts

[[routing]], [[planning-pattern]], [[single-vs-multi-agent]], [[coding-agents]],
[[crewai]], [[langgraph]]

## Sources

- OpenAI Agents SDK handoff documentation
- CrewAI architecture documentation
- LangGraph multi-agent patterns
