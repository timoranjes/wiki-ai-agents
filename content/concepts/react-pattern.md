---
title: ReAct Pattern
created: 2026-07-11
updated: '2026-05-28'
type: concept
tags:
  - react
  - planning
  - tool-use
sources:
  - https://arxiv.org/abs/2210.03629
  - https://python.langchain.com/
  - https://openai.github.io/openai-agents-python/
confidence: high
---

# ReAct Pattern

## Definition

ReAct, short for Reasoning and Acting, is an agent pattern that
interleaves internal reasoning with external actions in a single
execution loop. Rather than solving a task purely inside the model or
purely through scripted tool calls, a ReAct agent alternates between
deciding what to do, taking an action, observing the result, and
continuing from the updated state.

This pattern matters because many real tasks cannot be solved from
static context alone. Agents often need to search, inspect files, call
APIs, browse the web, or run code before they can answer correctly.

The pattern was introduced by Yao et al. in 2022 and has since become
one of the most widely adopted agent architectures. Its core insight is
simple but powerful: reasoning benefits from grounding in real-world
feedback, and acting benefits from deliberate planning before each step.

ReAct's name comes from the interleaving of Thought (reasoning) and
Action (tool use) traces, producing outputs like:

```
Thought: I need to find the population of Tokyo.
Action: Search[population of Tokyo]
Observation: Tokyo has approximately 14 million residents.
Thought: The population is about 14 million.
Action: Finish[14 million]
```

## Mechanics

A typical ReAct loop has three repeated elements:

1. **Reasoning step** — the agent interprets the current state and
   chooses the next move. This may involve recalling prior knowledge,
   analyzing tool outputs, or deciding that the task is complete.

2. **Action step** — the agent invokes a tool or performs an external
   operation. This could be a search query, a file read, an API call,
   a code execution, or any other interaction with the environment.

3. **Observation step** — the environment returns new evidence that
   updates the task state. The agent ingests this observation and feeds
   it into the next reasoning step.

The loop repeats until the agent reaches a stopping condition such as
task completion, failure, or a turn limit.

### Stopping conditions

Robust ReAct implementations define explicit stopping rules:

- **Task completion**: the agent produces a final answer using a
  designated "Finish" or "Answer" action.
- **Failure threshold**: the agent has exceeded a maximum number of
  turns without progress.
- **Repetition detection**: the agent recognizes it is cycling through
  the same actions and halts.
- **Human escalation**: the agent defers to a human operator when
  confidence is too low.

## Why It Works

ReAct improves reliability because it grounds reasoning in feedback from
the world. Pure chain-of-thought can drift when the model lacks fresh
evidence. Pure tool pipelines can become brittle when there is no
adaptive planning between tool calls. ReAct combines both.

It is especially effective for tasks that are uncertain, multi-step, or
stateful. Search, debugging, investigation, code repair, browser
automation, and retrieval-heavy question answering all fit this shape.

### Comparison to alternatives

- **Chain-of-thought (CoT)**: CoT reasons entirely internally without
  external feedback. It works well for tasks the model already knows
  but fails when the model needs information it does not have.

- **Program-of-thought (PoT)**: PoT generates executable code to solve
  reasoning tasks. It is powerful for mathematical problems but does not
  handle open-ended exploration well.

- **Plan-and-execute**: This pattern separates planning from execution
  into two distinct phases. It is more structured than ReAct but less
  adaptive when conditions change during execution.

- **[[prompt-chaining]]**: Prompt chaining composes fixed sequences of
  prompts. It is efficient for well-defined pipelines but cannot adapt
  to intermediate results.

ReAct sits between these extremes: it plans incrementally, one step at
a time, with feedback at each step.

## Trade-offs

ReAct is powerful, but it is not free:

- **Latency**: Each loop step may require another model call, adding
  round-trip time. A 5-step task could take 10-15 seconds or more.

- **Cost**: The reasoning trace and tool observations accumulate in
  context, increasing token consumption with each iteration.

- **Loop risk**: Agents can get stuck in loops if stopping rules are
  weak or if tool outputs are ambiguous.

- **Tool design dependency**: ReAct benefits from strong tool design
  and clear observation formatting. Poorly designed tools derail
  reasoning.

- **Context window pressure**: Long-running loops can exhaust the
  context window, forcing truncation of earlier reasoning steps.

- **Error propagation**: A single bad reasoning step can lead the agent
  down the wrong path for several subsequent steps.

## Framework Support

ReAct is so foundational that nearly every major agent framework
implements some variant:

- [[langchain]]: Has a dedicated ReAct agent implementation
  (`create_react_agent`) that chains a reasoning prompt with tool
  execution.

- [[langgraph]]: Provides a lower-level implementation where you
  explicitly define the ReAct loop as a graph with reasoning, action,
  and observation nodes.

- [[crewai]]: CrewAI agents can be configured to use a ReAct-style
  loop internally, with role-based delegation across multiple agents.

- [[autogen]]: AutoGen's conversational multi-agent approach can
  implement ReAct by having one agent reason and another act.

- [[openai-agents-sdk]]: The SDK's agent model with tool use and
  handoffs supports a ReAct loop natively.

- [[llamaindex]]: Query engines can incorporate ReAct-style reasoning
  over retrieved documents.

- [[google-adk]]: Supports ReAct patterns through built-in tool-calling
  and state management capabilities.

- [[anthropic]]: Claude models via the API support tool use with
  structured outputs, enabling ReAct loops with strong reasoning.

## When to Use

ReAct is the right pattern when:

- **The task requires external information** that the model does not
  have in its training data.

- **Multiple tool calls are needed** and the sequence depends on
  intermediate results.

- **The environment is uncertain** and the agent must adapt its plan
  based on observations.

- **You need transparency** into the agent's decision-making process,
  since each reasoning step is visible.

- **Debugging agent behavior** is important — the step-by-step trace
  makes it easy to identify where the agent went wrong.

When **not** to use ReAct:

- **Simple, single-step tasks** that can be solved with a direct tool
  call or prompt.

- **Latency-sensitive applications** where each additional model call
  adds unacceptable delay.

- **Cost-constrained scenarios** where the token budget is prohibitive.

- **Tasks with a known, fixed sequence** where [[prompt-chaining]] or
  plan-and-execute is more efficient.

- **Tasks requiring strict determinism** where adaptive reasoning
  introduces unwanted variability.

## Relationship to Broader Agent Patterns

ReAct serves as a foundation that other patterns build upon:

- [[planning-pattern]] extends ReAct by adding explicit multi-step
  planning before the loop begins, then replanning when conditions
  change.

- [[reflection-pattern]] can be inserted into the ReAct loop as an
  additional reasoning step where the agent critiques its own progress.

- [[tool-use-pattern]] is the mechanical layer that ReAct operates on
  top of — ReAct defines the loop structure, tool-use defines the
  capabilities.

- [[function-calling]] provides the structured interface through which
  ReAct agents invoke tools.

- [[routing]] can precede a ReAct loop by determining which set of
  tools and which reasoning strategy the agent should use.

- [[orchestrator-workers]] can decompose a complex task into subtasks,
  each handled by a ReAct agent with specialized tools.

- [[evaluation]] benefits from ReAct's transparent trace, which makes
  it possible to assess not just the final answer but the reasoning
  path that led to it.

## Notes

The ReAct paper demonstrates that the pattern significantly outperforms
both chain-of-thought and pure action baselines on knowledge-intensive
tasks. The key finding is that reasoning traces help the model stay
focused on relevant information, while observations prevent the model
from hallucinating facts.

In practice, many production agent systems use a ReAct variant even
when they do not explicitly name it. The pattern has become so pervasive
that it is effectively the default architecture for tool-using agents.

Variants of ReAct have emerged in subsequent research, including ReWOO
(which decouples reasoning from tool execution to reduce latency) and
Self-Ask (which structures the reasoning step as explicit subquestion
generation). These variants address specific trade-offs while preserving
the core ReAct insight.

## Related Concepts

[[tool-use-pattern]], [[function-calling]], [[planning-pattern]],
[[reflection-pattern]], [[prompt-chaining]], [[routing]],
[[orchestrator-workers]], [[evaluation]]

## Sources

- Yao et al., "ReAct: Synergizing Reasoning and Acting in Language
  Models"
- Agent framework documentation and implementations
