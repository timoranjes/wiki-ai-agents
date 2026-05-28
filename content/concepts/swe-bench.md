---
title: SWE-bench
created: 2026-07-11
updated: '2026-05-28'
type: concept
tags:
- benchmark
- evaluation
- code-generation
sources: [https://arxiv.org/abs/2310.06770, https://www.swebench.com/, https://www.swebench.com/lite]
confidence: high
---

# SWE-bench

## Definition

SWE-bench is a benchmark for evaluating whether AI systems can resolve real software engineering issues in real repositories. Each task combines a genuine GitHub issue with the corresponding codebase, and the evaluated system must generate a patch that fixes the issue and satisfies the test environment.

It became one of the most important benchmarks for [[coding-agents]] because it measures end-to-end engineering ability rather than isolated code completion. A model must understand the issue, inspect a codebase, choose what to edit, and survive execution-based verification.

## What It Measures

SWE-bench is useful because it tests several abilities at once:

- issue comprehension from natural language bug reports that may be vague, incomplete, or ambiguous
- codebase navigation and localization across large repositories with thousands of files
- patch generation across existing code with correct syntax, semantics, and style conventions
- compatibility with repository-specific test environments including dependency installation and build configuration
- iterative repair under execution feedback when initial patches fail tests or introduce regressions

That makes it a much better proxy for agentic software work than short synthetic coding tasks. The benchmark captures the full lifecycle of a real software engineering task, from reading a bug report to producing a verified fix.

## Benchmark Design

### Task Construction

Each SWE-bench task is built from a real GitHub pull request. The benchmark extracts the issue description, the repository state before the PR, and the golden patch that was ultimately merged. The evaluation system receives only the issue description and repository, and must independently discover what files to change and how.

Tasks are sourced from popular open-source Python projects, ensuring that the codebases are large enough to be realistic and the issues are varied enough to test different capabilities. The selection criteria aim to exclude trivially easy tasks while keeping the dataset manageable for evaluation.

### Verification Methodology

Patches are verified by applying them to the pre-PR repository state and running the repository's own test suite. A task is considered solved if the golden tests pass after applying the generated patch. This execution-based verification is much stronger than text-based similarity metrics because it tests whether the fix actually works, not whether it looks similar to the reference solution.

The verification harness must handle repository-specific setup including package installation, environment configuration, and test discovery. This infrastructure work is a significant part of running SWE-bench evaluations and represents a real-world challenge that coding agents also face.

### Dataset Composition

The benchmark draws tasks from multiple open-source Python projects across different domains including web frameworks, data science libraries, developer tools, and scientific computing. This diversity helps ensure that systems cannot overfit to a single codebase style or domain. Tasks vary in difficulty from simple one-line fixes to complex multi-file changes requiring understanding of architectural patterns and cross-module interactions.

## Variants and Reporting

The benchmark ecosystem includes different variants with different properties:

- **SWE-bench Classic** — the original full dataset with all tasks, representing the most comprehensive evaluation
- **SWE-bench Lite** — a curated subset of tasks selected for quality and representativeness, commonly used for faster evaluation and iteration
- **SWE-bench Verified** — tasks that have been manually validated to ensure the golden patch is correct and the tests are reliable, providing the highest confidence in evaluation results

Results are not directly comparable unless the setup is clearly specified. Several factors can significantly affect headline numbers:

- Retrieval strategy: systems that can effectively locate relevant code have a major advantage
- Available tools: agents with file editing, test execution, and search capabilities outperform text-only approaches
- Execution environment: successful environment setup is a prerequisite for any patch verification
- Pass criteria: different evaluation runs may use different definitions of what constitutes a passing solution

As a result, SWE-bench scores should be interpreted as system-level results, not just model-level results. A strong score can reflect better orchestration, better retrieval, better patch verification, or better environment handling in addition to model intelligence.

## Framework Support

Multiple agent frameworks and systems have been evaluated on SWE-bench. [[langchain]]-based coding agents can be configured to work against SWE-bench tasks by combining retrieval tools with file editing and test execution capabilities. The framework's tool abstraction layer makes it relatively straightforward to build agents that can navigate repositories, read files, generate patches, and run tests.

[[langgraph]] provides structured orchestration for the multi-step reasoning and tool-use sequences that SWE-bench tasks require. Its state-machine approach is well-suited for tracking the agent's progress through the task lifecycle: understanding the issue, locating relevant code, generating a patch, testing, and iterating.

[[autogen]] has been used in SWE-bench evaluations, particularly in multi-agent configurations where one agent handles issue analysis, another handles code modification, and a third handles test validation. The conversational architecture allows agents to debate approaches and review each other's work before committing changes.

The [[google-adk]] supports building SWE-bench agents through its tool-calling and state management primitives, enabling structured workflows that combine code retrieval, patch generation, and test execution.

Specialized systems like [[setupx]] address specific failure modes observed in SWE-bench evaluation, particularly around environment setup and dependency installation that can prevent agents from even starting to solve tasks. Environment setup is often cited as one of the biggest practical challenges in running SWE-bench at scale.

## When to Use

SWE-bench is valuable when:

- You need a rigorous, execution-based benchmark for coding agent capabilities
- You want to compare different agent architectures on a common task set with real repository complexity
- You are researching the gap between code completion and full engineering task completion
- You need to validate that a coding agent can handle the full lifecycle from issue understanding to verified fix
- You want to measure progress on repository-level code repair over time

SWE-bench is less appropriate when:

- You need to evaluate speed or cost efficiency, as the benchmark focuses on pass/fail rates rather than operational metrics
- You want to measure code quality, maintainability, or architectural soundness beyond test pass rates
- Your agent targets non-Python ecosystems without adapting the evaluation harness to other languages
- You need benchmarks for non-repair tasks like feature development, API design, or architecture planning
- You want to evaluate developer experience factors like interaction quality, explainability, or workflow integration

## Why It Matters for Agents

SWE-bench became central because coding agents are one of the clearest commercial applications of AI agents. Investors, researchers, and developer-tool buyers all want a simple way to compare systems that claim to act like software engineers.

For that reason, benchmark movement in SWE-bench often shapes market perception of which vendors are ahead in coding agents, even though the benchmark still omits important real-world dimensions such as speed, cost, maintainability, and workflow fit.

The benchmark has also driven significant research investment. Teams working to improve SWE-bench scores have developed novel approaches to code retrieval, iterative repair, environment handling, and test-driven development automation. These advances benefit the broader agent ecosystem even beyond SWE-bench itself.

The trajectory of SWE-bench results has been one of the most closely watched metrics in the AI industry. Rapid improvements in scores over a short period demonstrated that the benchmark was sensitive enough to capture meaningful advances in agent capability, while the gradual plateauing of gains highlighted the remaining challenges in complex repository repair.

## Limitations

SWE-bench should not be treated as the whole story. Several important limitations affect its applicability:

- It overweights repository repair under benchmark conditions that differ significantly from real developer workflows, where developers have access to documentation, colleagues, and broader context
- It does not fully capture product UX, latency, or cost efficiency — dimensions that matter enormously in production
- It may reward systems that are specifically optimized for benchmark-style execution rather than general engineering ability
- It does not guarantee that a high-scoring agent fits real enterprise workflows with code review, compliance, and integration requirements
- The Python-centric focus limits direct applicability to other language ecosystems without significant adaptation
- It measures fix correctness but not code quality, readability, or architectural soundness — a patch that passes tests may still be a poor engineering decision
- Context window constraints mean some large repositories cannot be fully loaded, forcing systems to rely on retrieval strategies that may miss relevant code
- The benchmark captures point-in-time fixes but not ongoing maintenance, refactoring, or the social aspects of software development

The right interpretation is that SWE-bench is a strong but incomplete signal. It should be combined with broader [[evaluation]] practices that measure additional dimensions such as developer satisfaction, integration with existing toolchains, and performance on production-like tasks.

## Relationship to Broader Evaluation

SWE-bench is part of a larger [[evaluation]] landscape for AI agents. While it focuses on repository-level code repair, other benchmarks address different capabilities:

- SWE-bench tasks typically require the [[react-pattern]] of alternating between reasoning and action — the agent reasons about what to do, takes an action (read a file, run a test), observes the result, and repeats
- Successful systems often incorporate [[planning-pattern]] elements for decomposing complex issues into manageable steps before executing changes
- The [[reflection-pattern]] is valuable for reviewing generated patches before test execution, catching obvious errors that would waste compute cycles
- [[function-calling]] accuracy directly affects performance, as agents must correctly invoke file-reading, file-editing, and test-execution tools
- [[tool-use-pattern]] effectiveness determines whether the agent can access the right tools at the right time during the repair process

For teams evaluating coding agents, SWE-bench should be one component of a broader assessment strategy that includes [[evaluation]] of real-world task performance, developer feedback, and operational metrics.

The benchmark's influence extends beyond its direct results. The methodology and infrastructure developed for SWE-bench evaluation — environment isolation, test harness construction, patch application and verification — have become reusable components that benefit the broader coding agent research community.

As the benchmark matures, the community is exploring extensions to additional language ecosystems, integration with continuous integration pipelines, and more nuanced scoring that accounts for partial fixes and code quality. These directions will make SWE-bench even more valuable as an evaluation tool for the next generation of coding agents.

The broader significance of SWE-bench is that it established execution-based evaluation as the gold standard for coding benchmarks, influencing how the entire field thinks about measuring agent capabilities beyond text-generation metrics.

## Related Concepts

[[coding-agents]], [[evaluation]], [[setupx]], [[function-calling]], [[react-pattern]], [[planning-pattern]], [[tool-use-pattern]], [[reflection-pattern]]

## Sources

- Princeton NLP SWE-bench paper and project documentation
- swebench.com
- Framework-specific SWE-bench evaluation reports
- Community analyses of SWE-bench results and methodology
