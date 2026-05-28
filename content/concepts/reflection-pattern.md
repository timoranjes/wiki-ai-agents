---
title: Reflection Pattern
created: 2026-05-28
updated: '2026-05-28'
type: concept
tags:
- reflection
- evaluation
- react
sources: [https://arxiv.org/abs/2303.11366, https://arxiv.org/abs/2305.10601, https://langchain-ai.github.io/langgraph/]
confidence: high
---

# Reflection Pattern

## Definition

The **Reflection** (or self-correction) pattern enables an agent to critique, evaluate,
and revise its own outputs before finalizing them. Instead of producing a single answer
and stopping, the agent enters a self-assessment phase where it reviews its work against
criteria, identifies flaws or gaps, and generates improved versions.

This pattern draws inspiration from human metacognition — the ability to think about
one's own thinking. In LLM agents, reflection is typically implemented as a separate
generation step (or a separate LLM call) that evaluates the primary output and suggests
corrections or enhancements.

## How It Works

The reflection loop has two main phases:

1. **Generation** — The agent produces an initial output (answer, code, plan, etc.).

2. **Reflection** — A critique phase where the agent (or a separate critic model)
   evaluates the output against explicit criteria:
   - Does it satisfy the user's requirements?
   - Are there factual errors or logical gaps?
   - Is the format correct?
   - Could it be more complete, concise, or accurate?

3. **Revision** — Based on the critique, the agent generates an improved output.

### Common Variants

- **Single reflection** — One generate → critique → revise cycle.
- **Iterative reflection** — Multiple rounds until the agent is satisfied or a max
  iteration count is reached.
- **Dual-LLM reflection** — A separate, often stronger model acts as the critic,
  providing more reliable feedback.
- **Self-reflection** — The same model critiques its own work (cheaper but less
  reliable due to self-bias).
- **External feedback reflection** — Incorporates user feedback or tool outputs
  (e.g., test results) into the critique.

### Pseudocode

```
output = generate(user_query)
for i in range(max_iterations):
    critique = reflect(output, criteria)
    if critique.score >= threshold:
        return output
    output = revise(output, critique)
return output
```

## When to Use

**Best suited for:**
- Code generation — where test execution provides concrete feedback for reflection
- Writing and summarization tasks — where quality is subjective and benefits from
  multiple passes
- Complex planning — where the initial plan may have gaps or unrealistic assumptions
- Safety-critical applications — where catching errors before delivery matters
- Tasks with clear evaluation criteria that can be articulated as critique prompts

**Trade-offs:**
- **Latency** — Each reflection cycle adds at least one additional LLM call.
- **Cost** — Multiple generation passes multiply token consumption.
- **Diminishing returns** — After 2-3 iterations, improvements often plateau.
- **Self-bias** — Models reviewing their own work tend to be overly lenient;
  dual-LLM setups mitigate this but increase cost.
- **Not ideal for** time-sensitive applications or trivial tasks where a single
  pass produces acceptable results.

## Related Concepts

- [[react-pattern]] — Reflection can be embedded within the ReAct loop to critique
  reasoning traces before committing to an action
- [[planning-pattern]] — Plans benefit greatly from reflection; reviewing a plan
  before execution catches decomposition errors early
- evaluation — Reflection is closely related to evaluation but is self-directed
  rather than externally administered
- [[tool-use-pattern]] — Tool outputs (e.g., test failures, compiler errors) provide
  concrete signals for reflection-driven revision
- prompt-chaining — Reflection is a specific two-stage chain (generate → critique → revise)

## Framework Support

- [[langchain]] supports reflection through its `Self-Correction` chain recipes
- [[langgraph]] excels at building reflection loops with explicit state and
  conditional routing between generation and critique nodes
- [[openai-agents-sdk]] enables reflection via its built-in agent loop with custom
  guard functions
- [[crewai]] supports reflection through task validation callbacks and custom
  review processes

## Key Considerations

The effectiveness of reflection depends on the quality of the critique. A vague
"review this" prompt yields shallow feedback. Effective reflection prompts specify:
- Concrete evaluation criteria
- Error categories to look for
- A scoring rubric or pass/fail threshold
- The desired format for the critique output
