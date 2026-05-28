---
title: Evaluation
created: 2026-05-28
updated: '2026-05-28'
type: concept
tags:
- evaluation
- benchmark
- quality
sources: [https://arxiv.org/abs/2305.10601, https://openai.github.io/openai-agents-python/]
confidence: high
---

# Evaluation

## Definition

**Evaluation** in the AI agent context refers to the systematic assessment of agent
outputs, behaviors, and performance against defined criteria. Unlike traditional
software testing with deterministic pass/fail outcomes, agent evaluation must handle
open-ended outputs, multi-step reasoning traces, and subjective quality dimensions.

Evaluation is distinct from [[reflection-pattern]] — reflection is self-directed
(self-correction during execution), while evaluation is externally administered
(assessment after or between executions). Both serve quality assurance but at
different stages of the agent lifecycle.

## Dimensions of Agent Evaluation

### Output Quality
- **Accuracy** — Does the output contain correct facts, valid code, or sound reasoning?
- **Completeness** — Does the output address all aspects of the input request?
- **Conciseness** — Is the output appropriately detailed without unnecessary verbosity?
- **Format compliance** — Does the output match the expected structure (JSON, markdown, etc.)?

### Behavioral Quality
- **Tool selection accuracy** — Does the agent choose the right tools for each step?
- **Error recovery** — How does the agent respond when tools fail or return unexpected results?
- **Efficiency** — Does the agent minimize unnecessary tool calls, API requests, or reasoning steps?
- **Safety** — Does the agent avoid harmful, biased, or policy-violating outputs?

### System Quality
- **Latency** — End-to-end response time from input to final output
- **Cost** — Token consumption and API call costs per task
- **Reliability** — Consistency of outputs across repeated executions of the same task
- **Scalability** — Performance under concurrent load

## Evaluation Methods

### Automated Metrics
- **Exact match** — For tasks with deterministic correct answers (math, code execution)
- **BLEU/ROUGE** — For text generation tasks (summarization, translation)
- **Pass@k** — For code generation: does at least one of k outputs pass the test suite?
- **Tool call accuracy** — Measures whether the agent selected the correct tool with correct arguments

### LLM-as-Judge
- An LLM evaluates outputs against rubrics, scoring dimensions like accuracy, completeness,
  and tone. Useful for subjective dimensions where automated metrics fall short.
- **Risk:** LLM judges have known biases (position bias, verbosity bias, self-preference)
  that must be controlled for.

### Human Evaluation
- Gold standard for quality assessment but expensive and slow.
- Best used for: final validation, rubric development, and calibrating automated metrics.

### Benchmark Suites
- **[[swe-bench]]** — Real GitHub issues, pass/fail based on test execution
- **GAIA** — General AI Assistant benchmark requiring multi-step tool use
- **AgentBench** — Multi-environment benchmark testing agents across OS, web, and database tasks
- **HELM** — Holistic evaluation framework covering accuracy, robustness, fairness, and more

## Evaluation in Agent Development

Evaluation is not a one-time activity — it's integrated throughout the agent lifecycle:

1. **Development** — Unit tests for individual tools, integration tests for agent loops
2. **Regression** — Automated evaluation suites run on every code change
3. **A/B testing** — Comparing different prompts, models, or architectures in production
4. **Continuous monitoring** — Tracking output quality metrics over time to detect degradation

## Relationship to Other Concepts

- [[reflection-pattern]] — Self-directed quality improvement during execution; evaluation
  is externally administered assessment
- [[swe-bench]] — A specific benchmark suite for code-related agent tasks
- [[coding-agents]] — These agents require rigorous evaluation to ensure code correctness
- [[tool-use-pattern]] — Tool selection accuracy is a key evaluation dimension

## Related Concepts

[[reflection-pattern]], [[swe-bench]], [[coding-agents]], [[tool-use-pattern]],
[[persona2web]]

## Sources

- LLM-as-a-Judge research (Zheng et al.)
- OpenAI Agents SDK evaluation patterns
- AgentBench benchmark documentation
