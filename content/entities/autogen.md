---
title: AutoGen
created: 2026-05-28
updated: '2026-05-28'
type: entity
tags:
- autogen
- multi-agent
- tool-use
- code-generation
- evaluation
- open-source
sources: [https://github.com/microsoft/autogen, https://microsoft.github.io/autogen/stable/]
confidence: high
---

# AutoGen

## Overview

AutoGen is an open-source framework from Microsoft Research for building multi-agent applications through configurable, conversational agent patterns. It enables developers to create agents that can converse with each other, humans, and tools to solve tasks collaboratively. Originally released as research software, AutoGen has evolved into a production-ready framework with support for diverse multi-agent topologies including pair programming, group discussions, and hierarchical decision-making.

AutoGen's distinguishing feature is its conversational paradigm — agents communicate via message passing, and the framework handles turn-taking, termination conditions, and tool invocation. This makes it particularly effective for code generation, debugging, and tasks where iterative refinement through dialogue yields better results than single-pass execution.

## Key Facts

| Attribute | Value |
|---|---|
| Creator | Microsoft Research |
| Organization | Microsoft |
| Initial Release | August 2023 |
| License | MIT / Apache 2.0 |
| Primary Language | Python (primary), .NET |
| GitHub Stars | 30,000+ |
| Package Manager | `pip install autogen-agentchat` |

## Architecture Highlights

### ConversableAgent

The core abstraction is `ConversableAgent`, which supports:

- **LLM-backed inference**: Integrates with openai, Azure OpenAI, [[anthropic]], and open-source models
- **Code execution**: Built-in Docker-based code executor for safely running generated code
- **Tool/function calling**: OpenAI-compatible function calling and custom tool registration
- **Human input**: Native support for requesting human feedback during conversations
- **State management**: Maintains conversation history and can summarize to manage context windows

### Multi-Agent Conversation Patterns

AutoGen supports several conversation topologies:

- **Two-Agent Chat**: A user-proxy agent and an assistant agent alternate turns — the classic pair programming setup where one generates code and the other executes and provides feedback
- **Group Chat**: Multiple agents participate in a shared conversation with a group chat manager that selects the next speaker based on rules or LLM-based selection
- **Nested Chats**: One agent can spawn sub-conversations with other agents, enabling hierarchical delegation patterns
- **Sequential Workflows**: Agents execute in a predefined order, passing results along a chain

### AgentChat Framework (AutoGen 0.4+)

The newer `autogen-agentchat` library introduces a redesigned API:

- **Team**: A collection of agents with a defined termination condition
- **SingleAgentTeam**: Wraps a single agent for simple use cases
- **SelectorGroupChat**: LLM-based speaker selection determines who speaks next
- **RoundRobinGroupChat**: Deterministic turn-taking order
- **Termination Conditions**: Flexible stop criteria including max turns, text matching, or custom predicates

### Code Execution and Safety

AutoGen's code execution capabilities are a first-class feature:

- **Docker-based isolation**: Generated code runs in containerized environments for safety
- **Local execution**: Optional direct execution for trusted environments
- **Result feedback**: Execution output (stdout, stderr, return values) is fed back into the conversation loop
- This makes AutoGen particularly strong for code-generation and debugging workflows.

### Model Context and Caching

- **Model clients**: Unified interface for multiple LLM providers with fallback and retry logic
- **Caching**: Response caching to reduce API costs during iterative development
- **Cost tracking**: Built-in usage accounting across all agents in a conversation

### Integration with Microsoft Ecosystem

- **Azure AI**: Native integration with Azure OpenAI and Azure AI Search
- **Semantic Kernel** — Can be composed with semantic-kernel for enterprise application patterns
- **Microsoft 365**: Emerging connectors for Office and Teams integration

### Code Example

```python
from autogen_agentchat.agents import AssistantAgent
from autogen_agentchat.teams import RoundRobinGroupChat
from autogen_agentchat.conditions import MaxMessageTermination

assistant = AssistantAgent(
    name="assistant",
    model_client=openai_client,
    system_message="You are a helpful coding assistant.",
)

team = RoundRobinGroupChat(
    [assistant],
    termination_condition=MaxMessageTermination(max_messages=5),
)

result = await team.run(task="Write a Python function to sort a list.")
```

## Relationships

- **LangChain** — [[langchain]] provides general LLM application primitives; AutoGen focuses specifically on conversational multi-agent patterns rather than general chain composition.
- **LangGraph** — [[langgraph]] offers explicit graph-based state machines for custom agent control flow, while AutoGen uses conversation-driven control flow with speaker selection; both support multi-agent patterns with different abstractions.
- **CrewAI** — [[crewai]] uses role-based declarative task assignment with sequential or hierarchical processes, while AutoGen emphasizes conversational dynamics where agents negotiate and refine solutions through dialogue.
- **Code Generation** — AutoGen's built-in code execution loop makes it particularly effective for code-generation tasks where agents write, run, and iteratively fix code.
- **Tool Use** — AutoGen agents implement tool-use through function calling and code execution, with results fed back into the conversation for iterative refinement.
- **Evaluation** — AutoGen's multi-agent debate patterns can serve as an evaluation mechanism, where multiple agents critique each other's outputs.
- **OpenAI Agents SDK** — openai-agents is OpenAI's native framework for agent building; AutoGen is model-agnostic and focuses on multi-agent conversation patterns rather than single-agent orchestration.

## Related Concepts

[[crewai]], [[langgraph]], [[coding-agents]], [[tool-use-pattern]]

## Sources

- Microsoft Research AutoGen repository
- AutoGen documentation
