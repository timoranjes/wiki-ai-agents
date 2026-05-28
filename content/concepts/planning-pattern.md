---
title: Planning Pattern
created: 2026-05-28
updated: '2026-05-28'
type: concept
tags:
- planning
- react
sources: [https://arxiv.org/abs/2205.11916, https://langchain-ai.github.io/langgraph/, https://openai.github.io/openai-agents-python/]
confidence: high
---

# Planning Pattern

## Definition

The **Planning** (or subgoal decomposition) pattern enables an agent to break complex
tasks into smaller, manageable subgoals and execute them in a structured sequence.
Rather than attempting to solve a problem end-to-end in one pass, the agent first
creates a plan — a structured breakdown of steps — and then follows, adapts, or
replans as it progresses.

Planning is fundamental to complex problem-solving. Research shows that LLMs produce
significantly better results on multi-step tasks when given explicit planning phases
compared to direct generation, especially for tasks requiring math, coding, or
multi-hop reasoning.

## How It Works

The planning pattern involves three core phases:

### 1. Plan Generation

The agent receives a high-level task and decomposes it into ordered subgoals. This
can take several forms:

- **Linear plan** — A simple ordered list of steps to execute sequentially.
- **DAG plan** — A directed acyclic graph where some subgoals can be executed in
  parallel and others have dependencies.
- **Hierarchical plan** — A tree structure where top-level goals decompose into
  sub-goals, which may further decompose.

### 2. Plan Execution

The agent executes subgoals one at a time (or in parallel where possible), collecting
intermediate results. Each subgoal may itself be a complex task delegated to a
sub-agent or solved with another pattern like [[react-pattern]].

### 3. Replanning

If execution reveals that the plan is wrong or incomplete (e.g., a subgoal fails,
new information changes requirements), the agent revises the plan and continues.
This adaptive capability distinguishes robust planning from brittle pre-computation.

### Pseudocode

```
plan = generate_plan(task, decomposition_strategy="linear")
for step in plan.steps:
    result = execute(step)
    if step.needs_verification:
        feedback = verify(result)
        if feedback.failed:
            plan = replan(plan, step, feedback)
            restart_from(plan.next_affected_step)
return aggregate_results(plan)
```

## When to Use

**Best suited for:**
- Multi-step tasks where the path to solution is not immediately obvious
- Tasks with dependencies between sub-steps
- Problems that benefit from structured decomposition (software development,
  research workflows, data pipelines)
- Scenarios where parallel execution of independent subgoals can save time
- Tasks requiring intermediate checkpoints or human-in-the-loop review

**Trade-offs:**
- **Overhead** — Planning adds an upfront cost; for simple tasks, it may be
  unnecessary overhead compared to direct execution.
- **Plan fragility** — Rigid plans break when assumptions change; robust
  implementations need replanning logic.
- **Decomposition quality** — The agent's ability to create good plans depends
  on prompt quality and model capability; poor decomposition leads to inefficient
  or incorrect execution.
- **Complexity** — Managing plan state, dependencies, and replanning adds
  architectural complexity compared to simpler patterns.

## Related Concepts

- [[react-pattern]] — ReAct interleaves thinking and acting; planning separates
  them into distinct phases. ReAct can be viewed as implicit planning.
- [[reflection-pattern]] — Reflection can be applied to review a plan before
  execution, catching decomposition errors early.
- [[tool-use-pattern]] — Planned subgoals often translate into specific tool
  calls; planning determines what tools to use and in what order.
- [[routing]] — Routing decides which path or agent handles a task; planning
  determines the sequence of steps within a chosen path.

## Framework Support

- [[langgraph]] is particularly well-suited for planning with its explicit graph
  structure, allowing nodes for plan generation, execution, and replanning with
  conditional edges between them.
- [[langchain]] offers planning through its LLM chain abstractions and the
  Plan-and-Execute agent pattern.
- [[crewai]] implements planning through hierarchical task delegation where
  manager agents create and distribute subtasks to worker agents.
- [[openai-agents-sdk]] supports planning through its handoff mechanism and
  multi-agent orchestration patterns.
- [[autogen]] enables planning through group chat orchestration where agents
  propose and refine plans collaboratively.

## Planning Strategies

| Strategy        | Description                                    | Best For              |
|-----------------|------------------------------------------------|-----------------------|
| Linear          | Ordered sequence of steps                      | Simple workflows      |
| Hierarchical    | Tree of nested subgoals                        | Complex multi-level   |
| Least-to-most   | Start simple, progressively add complexity     | Math, reasoning       |
| Tree-of-thoughts| Explore multiple plan branches, backtrack      | Search, optimization  |
