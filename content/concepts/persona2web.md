---
title: Persona2Web
created: 2026-05-28
updated: '2026-05-28'
type: concept
tags:
  - benchmark
  - evaluation
  - research-paper
sources:
  - https://arxiv.org/abs/2602.17003
confidence: high
---

# Persona2Web

Persona2Web is a benchmark for personalized web agents that tests whether
agents can resolve ambiguous tasks by using user history instead of relying
only on explicit instructions.

## Why it matters

Most [[coding-agents]] and web agents are evaluated as if the user intent
is fully specified. Persona2Web highlights a more realistic condition:
users often leave preferences implicit, so the agent must infer context
from prior behavior and history.

In everyday web interaction, users rarely spell out every constraint.
A request like "book a flight to New York" implicitly depends on past
travel patterns, budget preferences, airline loyalty, seat class
preferences, and schedule habits. An agent that ignores this context
will produce technically correct but practically useless results.

Persona2Web formalizes this gap by creating tasks where the correct
answer depends on knowing user history that is not restated in the task
prompt. This forces agents to either retrieve relevant history or ask
clarifying questions rather than guessing from the current instruction
alone.

This matters for agent design because personalization is not a cosmetic
feature — it is a core capability that determines whether users trust the
agent enough to rely on it for real tasks. An agent that consistently
ignores user preferences will quickly be abandoned.

## What it evaluates

Persona2Web is built around three ingredients:

- **User histories** that reveal preferences over long time spans.
  These include past web interactions, purchase decisions, search queries,
  and stated preferences that collectively form a user profile. The
  histories contain both explicit and implicit preferences.

- **Ambiguous web tasks** that require preference inference. The tasks
  are deliberately underspecified so that multiple technically valid
  answers exist, but only one aligns with the user's inferred preferences.

- **A reasoning-aware evaluation framework** for fine-grained assessment.
  Rather than simple pass/fail scoring, the benchmark evaluates whether
  the agent correctly identified which preference dimension was relevant,
  retrieved the right historical evidence, and applied it appropriately.

## Framework Support

Persona2Web is a benchmark, not a framework, but it influences how
several agent frameworks handle personalization:

- [[langchain]]: LangChain's memory modules (conversation buffer, entity
  memory, vector-backed stores) provide the infrastructure for storing
  and retrieving user history. A LangChain agent could implement the
  "clarify-to-personalize" flow that Persona2Web tests.

- [[langgraph]]: Graph-based orchestration naturally supports a branching
  flow where the agent first retrieves user history, evaluates ambiguity,
  and then either acts directly or asks a clarifying question.

- [[autogen]]: Multi-agent conversations can dedicate one agent to
  preference inference (the "personalization agent") while another handles
  web execution, creating a natural separation of concerns.

- [[openai-agents-sdk]]: The SDK's agent model with structured outputs
  and guardrails can implement a two-phase flow: history retrieval and
  preference inference as one agent step, web action as another.

- [[crewai]]: A CrewAI crew can include a "Researcher" agent that
  retrieves user history and a "Manager" agent that resolves ambiguity
  and delegates web tasks.

- **Custom agents**: The clarify-to-personalize pattern is framework-
  agnostic. Any agent system with persistent storage and the ability to
  generate clarifying questions can implement it.

## When to Use

Persona2Web concepts apply when:

- **Building web agents that interact with real users**. Personalization
  is not optional in production — users expect agents to remember their
  preferences and apply them automatically.

- **Your agents handle ambiguous requests frequently**. Tasks like
  shopping, booking, content recommendations, and information retrieval
  often come with implicit constraints.

- **You want to benchmark personalization separately**. Persona2Web
  isolates preference inference from raw web execution, making it
  possible to measure each capability independently.

- **Your agent has access to user history**. The benchmark assumes the
  agent can access past interactions, preferences, and behavioral data.

When **not** to use Persona2Web concepts:

- Internal or enterprise agents where all tasks are well-specified and
  standardized.

- Agents that operate on behalf of a single user with fixed, documented
  preferences that never change.

- Tasks where ambiguity is undesirable or unsafe (e.g., financial
  transactions, medical decisions) and explicit confirmation is always
  required.

- Scenarios where user history is unavailable due to privacy regulations
  or technical limitations.

## Agent-design implications

Persona2Web is useful for thinking about advanced web-agent behavior:

- **Personalization is a missing dimension** in many web-agent benchmarks.
  SWE-bench measures code repair, web browsing benchmarks measure
  navigation accuracy, but few measure whether the agent understood which
  result the user actually wanted.

- **Clarification and history use should be treated as part of the task**,
  not as optional polish. An agent that asks "which airline do you
  prefer?" before booking is demonstrating the exact behavior Persona2Web
  rewards.

- **Strong web agents need both execution ability and preference
  inference**. A technically capable agent that picks the wrong hotel is
  worse than a less capable agent that picks the right one.

## Relationship to Broader Agent Patterns

Persona2Web intersects with several core patterns:

- It embodies a specialized [[react-pattern]] loop where reasoning
  includes preference inference, action involves web interaction, and
  observation evaluates whether the result matches inferred preferences.

- It extends [[routing]] by requiring the agent to route between "act
  directly" and "ask for clarification" based on ambiguity detection.

- It complements [[evaluation]] by providing a focused benchmark for
  personalization capability that other benchmarks ignore.

- It demonstrates how [[planning-pattern]] can incorporate user
  preference as a constraint in task decomposition.

- It relates to [[orchestrator-workers]] where one worker handles
  preference inference and another handles execution.

- It connects to [[tool-use-pattern]] because web agents act through
  browsers and tools, and preference inference guides which tools to use.

## Notes

The paper frames the task as a "clarify-to-personalize" problem: the
agent should use prior context to disambiguate intent rather than waiting
for perfectly explicit instructions. This is a significant departure from
most benchmark design, which assumes the task description is complete and
unambiguous.

The reasoning-aware evaluation framework is particularly valuable because
it separates the quality of the agent's preference inference from the
quality of its web execution. This allows researchers and engineers to
diagnose whether failures come from poor personalization, poor execution,
or both.

The benchmark's design also has implications for privacy: if agents are
expected to use user history for personalization, then the storage,
retrieval, and use of that history must be handled with appropriate care.
Agent systems that implement clarify-to-personalize flows should include
privacy safeguards and user controls.

## Related Concepts

[[tool-use-pattern]], [[reflection-pattern]], [[coding-agents]],
[[swe-bench]], [[evaluation]], [[react-pattern]], [[routing]],
[[planning-pattern]], [[orchestrator-workers]]

## Sources

- Persona2Web paper: https://arxiv.org/abs/2602.17003
