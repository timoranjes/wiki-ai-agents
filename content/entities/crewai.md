---
title: CrewAI
created: 2026-05-28
updated: '2026-05-28'
type: entity
tags:
- crewai
- multi-agent
- orchestrator-workers
- tool-use
- planning
- open-source
sources: [https://github.com/crewAIInc/crewAI, https://docs.crewai.com/]
confidence: high
---

# CrewAI

## Overview

CrewAI is an open-source framework for orchestrating role-based multi-agent teams. It allows developers to define agents with specific roles, goals, and tools, then organize them into crews that collaborate on tasks through structured processes. Created by João Moura, CrewAI emphasizes a declarative, high-level approach to multi-agent design — developers describe what agents should do, and the framework manages how they coordinate.

CrewAI is particularly well-suited for workflows where domain expertise can be decomposed into distinct roles (e.g., researcher, writer, reviewer) that work together sequentially or hierarchically to produce structured outputs.

## Key Facts

| Attribute | Value |
|---|---|
| Creator | João Moura |
| Organization | CrewAI Inc. |
| Initial Release | 2023 |
| License | MIT |
| Primary Language | Python |
| GitHub Stars | 25,000+ |
| Package Manager | `pip install crewai` |

## Architecture Highlights

### Agent Abstraction

The core building block is the `Agent`, defined by:

- **Role**: A descriptive title (e.g., "Senior Research Analyst") that shapes the agent's system prompt
- **Goal**: The objective the agent works toward (e.g., "Uncover trends in emerging technology markets")
- **Backstory**: Contextual narrative that further refines the agent's persona and behavior
- **Tools**: A list of tools the agent can use, such as web search, file I/O, or custom Python functions
- **LLM**: The language model backing the agent (supports openai, [[anthropic]], google, and open-source models via LiteLLM)
- **Memory**: Optional short-term and long-term memory for retaining context across tasks

### Task Abstraction

Tasks define the work agents perform:

- **Description**: Detailed instructions for what needs to be done
- **Expected Output**: Specification of the deliverable format
- **Agent**: The assigned agent (or left unassigned for crew-level routing)
- **Tools**: Task-specific tools that override or extend the agent's default toolset
- **Context**: Input from upstream tasks that this task depends on

### Crew and Process

A `Crew` bundles agents and tasks together with a process type:

- **Sequential**: Tasks execute in order, with outputs passed from one to the next. This implements the prompt-chaining pattern at the agent level.
- **Hierarchical**: A manager agent (the ` CrewAI Manager`) decomposes work, delegates to subordinate agents, and synthesizes results. This follows the hierarchical architecture pattern.
- **Consensual**: Agents discuss and vote on outputs before finalizing, enabling quality-through-consensus workflows.

The `Process` class encapsulates the execution strategy, handling task assignment, inter-agent communication, and output aggregation.

### Training and Memory

CrewAI supports agent improvement over time:

- **Short-term memory**: Stored in-memory during a single crew execution for context continuity
- **Long-term memory**: Persisted across executions using an embedding-based storage system
- **Training**: Agents can be "trained" by providing feedback on task outputs, which updates their behavioral patterns

### Tool Ecosystem

CrewAI integrates with a growing tool ecosystem:

- Built-in tools for web search, file operations, code execution, and API calls
- Compatibility with [[langchain]] tools, allowing reuse of LangChain's extensive tool library
- Custom tool creation via decorators for wrapping arbitrary Python functions

### CLI and Observability

- **CrewAI CLI**: Scaffold new projects, run crews, and manage deployments from the command line
- **CrewAI+**: Managed platform with dashboards, monitoring, and team collaboration features
- Integration with tracing tools for debugging multi-agent execution flows

### Code Example

```python
from crewai import Agent, Task, Crew

researcher = Agent(
    role="Research Analyst",
    goal="Analyze market trends",
    backstory="Expert in data analysis and market research",
    tools=[search_tool],
)

writer = Agent(
    role="Content Writer",
    goal="Write comprehensive reports",
    backstory="Experienced technical writer",
)

task = Task(
    description="Research and write a market analysis report",
    agent=researcher,
    expected_output="A detailed markdown report",
)

crew = Crew(
    agents=[researcher, writer],
    tasks=[task],
    process=Process.sequential,
)

result = crew.kickoff()
```

## Relationships

- **LangChain** — [[langchain]] tools can be used within CrewAI agents; CrewAI provides higher-level orchestration while LangChain offers lower-level LLM primitives.
- **LangGraph** — [[langgraph]] provides explicit graph-based control flow for custom agent topologies, while CrewAI uses a declarative role-and-task model; they represent different points on the abstraction spectrum.
- **AutoGen** — [[autogen]] emphasizes conversational patterns between agents with group chat, while CrewAI focuses on role-based task delegation and structured output production.
- **Orchestrator-Workers** — CrewAI's hierarchical process directly implements the orchestrator-workers pattern, with the manager agent routing work to specialized worker agents.
- **Tool Use** — CrewAI agents implement tool-use as a core capability, selecting and invoking tools based on task requirements.
- **Planning** — The hierarchical process involves implicit planning as the manager agent decomposes complex tasks into sub-tasks for delegation.

## Related Concepts

[[langchain]], [[langgraph]], [[autogen]], [[planning-pattern]]

## Sources

- CrewAI official documentation
- CrewAI GitHub repository
