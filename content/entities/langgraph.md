---
title: LangGraph
created: 2026-05-28
updated: '2026-05-28'
type: entity
tags:
- langgraph
- multi-agent
- memory
- tool-use
- open-source
sources: [https://langchain-ai.github.io/langgraph/, https://github.com/langchain-ai/langgraph]
confidence: high
---

# LangGraph

## Overview

LangGraph is a library for building stateful, multi-actor applications with LLMs, developed by the same team behind [[langchain]]. It extends LangChain's composition primitives by introducing a graph-based state machine model where nodes represent computational steps and edges define control flow, including cycles. This makes it ideal for building agents that require loops, branching, human-in-the-loop interaction, and persistent state across multiple turns.

While LangChain's chain abstraction excels at linear, directed-acyclic workflows, LangGraph addresses the gap for applications that need cyclical execution — such as agents that must retry, reflect, plan-and-replan, or maintain long-running conversational state.

## Key Facts

| Attribute | Value |
|---|---|
| Creator | LangChain Inc. (Harrison Chase and team) |
| Initial Release | Early 2024 |
| License | MIT |
| Primary Language | Python (primary), TypeScript/JavaScript |
| Dependency | Built on top of [[langchain]] |
| Package Managers | `pip install langgraph`, `npm install @langchain/langgraph` |

## Architecture Highlights

### Graph-Based State Machine

LangGraph's core abstraction is a **directed graph** where:

- **Nodes** are functions that receive and return a state dictionary. Each node represents a discrete computational step (e.g., "call the LLM", "invoke a tool", "summarize conversation").
- **Edges** define transitions between nodes. They can be static (always go from A to B) or conditional (route based on state values).
- **State** is a typed schema (typically a Pydantic model or TypedDict) that flows through the graph, accumulating and transforming data at each step.
- **Channels** are the mechanism by which state is read and written, supporting reducers (append, replace, accumulate) for different merge strategies.

### StateGraph API

The primary construction API uses `StateGraph`:

```python
from langgraph.graph import StateGraph, END

class AgentState(TypedDict):
    messages: Annotated[list, add_messages]
    next_action: str

builder = StateGraph(AgentState)
builder.add_node("agent", call_llm)
builder.add_node("tools", execute_tools)
builder.add_edge("agent", "tools")
builder.add_conditional_edges("tools", route_back)
builder.add_edge("tools", END)
graph = builder.compile()
```

The `add_messages` reducer demonstrates LangGraph's annotation system — it specifies how new messages should be merged into existing state (appending rather than replacing).

### Checkpointing and Persistence

LangGraph provides built-in state persistence via checkpointer backends:

- **MemorySaver**: In-memory checkpoint store for development and testing
- **SQLite checkpointer**: File-based persistence for local applications
- **Postgres checkpointer**: Production-grade persistent storage

Checkpoints enable:
- Resuming interrupted conversations
- Human-in-the-loop workflows where execution pauses for approval
- Debugging by replaying execution history
- Multi-session state management

### Human-in-the-Loop

LangGraph natively supports interrupting execution at defined breakpoints:

- **`interrupt_before` / `interrupt_after`**: Pause the graph before or after specific nodes
- **`Command(resume=...)`**: Resume execution with additional data from a human reviewer
- This pattern is essential for production agent systems where LLM actions require oversight before execution.

### Multi-Agent Patterns

LangGraph supports several multi-agent architectures:

- **Supervisor/Workers**: A central routing node delegates to specialized worker nodes
- **Hierarchical Teams**: Nested subgraphs allow composing teams within teams
- **Agent Swarms**: Parallel execution of independent agents with coordination points
- **Handoff**: Passing control between specialized agents in a conversation

These patterns align with the orchestrator-workers and hierarchical architectures documented in the agent architecture taxonomy.

### LangGraph Platform

Beyond the open-source library, LangChain Inc. offers LangGraph Platform with:
- LangGraph Studio: Visual IDE for building and debugging agent graphs
- LangGraph Cloud: Managed deployment with built-in monitoring and scaling
- LangSmith integration: Tracing and evaluation across LangGraph executions

## Relationships

- **LangChain** — [[langchain]] provides the foundational model, prompt, and tool abstractions that LangGraph builds upon; LangGraph can be viewed as the orchestration layer for LangChain components.
- **CrewAI** — [[crewai]] takes a role-based, declarative approach to multi-agent orchestration, whereas LangGraph provides lower-level graph primitives for custom agent topologies.
- **AutoGen** — [[autogen]] focuses on conversational multi-agent patterns with built-in group chat; LangGraph offers more explicit control flow and state management.
- **ReAct Pattern** — LangGraph commonly implements the react pattern through conditional routing between reasoning nodes and tool-execution nodes.
- **Planning** — LangGraph's ability to model cycles makes it well-suited for planning patterns where agents generate plans, execute steps, and replan on failure.
- **Memory** — LangGraph's checkpoint system provides a form of memory persistence that outlasts individual conversations.

## Related Concepts

[[langchain]], [[planning-pattern]], [[react-pattern]], [[reflection-pattern]]

## Sources

- LangGraph official documentation
- LangGraph GitHub repository
