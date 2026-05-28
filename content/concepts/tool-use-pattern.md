---
title: Tool Use Pattern
created: 2026-05-28
updated: '2026-05-28'
type: concept
tags:
- tool-use
- react
- planning
sources: [https://platform.openai.com/docs/guides/function-calling, https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/overview, https://python.langchain.com/]
confidence: high
---

# Tool Use Pattern

## Definition

The **Tool Use** (or function calling) pattern enables an LLM to invoke external
capabilities — APIs, databases, code interpreters, search engines, or any
programmatic function — to extend its abilities beyond text generation. Instead of
relying solely on parametric knowledge, the agent dynamically queries the external
world and incorporates real-time results into its responses.

## How It Works

The tool use pattern operates through a structured interaction loop:

### 1. Tool Definition

Tools are defined as structured schemas that describe:
- **Name** — A unique identifier for the tool
- **Description** — What the tool does (used by the LLM to decide when to call it)
- **Parameters** — Input schema (JSON Schema or equivalent) specifying required
  and optional arguments
- **Return type** — The format of the tool's output

### 2. Tool Selection

The LLM is presented with available tool definitions and decides — based on the
current context — which tool to call and with what arguments. Modern models support
this natively through:
- **Function calling APIs** (OpenAI, Anthropic, Google) — The model outputs
  structured tool-call objects instead of free text.
- **ReAct-style prompts** — The model outputs text-based tool calls parsed by the
  framework.

### 3. Tool Execution

The framework intercepts the tool call, executes the corresponding function, and
returns the result to the LLM as an observation.

### 4. Result Integration

The LLM incorporates the tool output into its reasoning and either:
- Produces a final answer using the gathered information
- Makes additional tool calls if more data is needed

### Pseudocode

```
tools = define_tools([...schemas...])
messages = [system_prompt, user_query]
while True:
    response = llm(messages, tools=tools)
    if response.tool_calls:
        for call in response.tool_calls:
            result = execute_tool(call.name, call.arguments)
            messages.append(tool_result_message(call.id, result))
    else:
        return response.text
```

## When to Use

**Best suited for:**
- Tasks requiring real-time or external data (web search, database queries, API calls)
- Computation-heavy tasks where the LLM's math capabilities are insufficient
- Actions that require side effects (sending emails, updating records, deploying code)
- Workflows that integrate with existing systems and services
- Any task where parametric knowledge alone is inadequate or outdated

**Trade-offs:**
- **Complexity** — Tool schemas must be carefully designed; poor descriptions lead
  to incorrect tool selection or malformed arguments.
- **Error handling** — Tools can fail, timeout, or return unexpected results. Robust
  implementations need retry logic and error feedback.
- **Security** — Exposing tools to LLMs requires careful permission management and
  input sanitization to prevent unintended actions.
- **Cost** — Each tool call adds latency and the additional LLM rounds consume tokens.
- **Model dependency** — Native function calling requires models that support it;
  older models need workarounds like ReAct-style text parsing.

## Related Concepts

- [[react-pattern]] — ReAct is the canonical pattern that structures tool use within
  a reasoning loop (Thought → Action → Observation).
- [[planning-pattern]] — Planning determines which tools to call and in what order;
  tool use is the execution mechanism for plans.
- [[reflection-pattern]] — Reflection can validate tool outputs (e.g., "did this
  search actually answer my question?") before proceeding.
- [[routing]] — Routing decides which tool or agent handles a request; tool use
  is the mechanism the chosen path employs.

## Framework Support

Virtually all modern agent frameworks support tool use:

- [[langchain]] pioneered the Tools abstraction with `@tool` decorators and built-in integrations
- [[openai-agents-sdk]] provides native function calling with automatic schema generation
- [[langgraph]] models tool use as graph nodes with conditional routing and state management
- [[crewai]] equips agents with custom tools for autonomous task execution
- [[autogen]] supports tool use through function registration in group chats
- google-adk integrates tool calling via Google's function-calling API with Gemini models

## Tool Design Best Practices

- **Clear descriptions** — Vague tool descriptions lead to missed or incorrect calls
- **Minimal parameters** — Only require what's necessary
- **Idempotent by default** — Prefer tools that can be safely retried
- **Rich error messages** — Helpful errors enable self-correction
- **Version tools** — Avoid breaking agents when tool behavior evolves
