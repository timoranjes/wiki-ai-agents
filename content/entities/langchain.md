---
title: LangChain
created: 2026-05-28
updated: '2026-05-28'
type: entity
tags:
- langchain
- tool-use
- rag
- prompt-chaining
- open-source
sources: [https://python.langchain.com/, https://github.com/langchain-ai/langchain]
confidence: high
---

# LangChain

## Overview

LangChain is an open-source framework for building applications powered by large language models. It provides a standardized interface for composing chains of LLM calls, integrating external tools, and managing state across multi-step workflows. Originally released in 2022, it has become one of the most widely adopted LLM application frameworks in the ecosystem.

LangChain's core philosophy is composability — it breaks down LLM interactions into modular components (prompts, models, parsers, tools, memory) that can be combined flexibly. This makes it suitable for everything from simple Q&A bots to complex rag pipelines and tool-use agents.

## Key Facts

| Attribute | Value |
|---|---|
| Creator | Harrison Chase |
| Organization | LangChain Inc. |
| Initial Release | October 2022 |
| License | MIT |
| Primary Language | Python (primary), TypeScript/JavaScript |
| GitHub Stars | 90,000+ |
| Package Managers | `pip install langchain`, `npm install langchain` |

## Architecture Highlights

### Core Abstractions

LangChain is built around several foundational abstractions:

- **Models**: Unified interfaces for openai chat models, [[anthropic]] models, google models, and dozens of others via a common API. The framework normalizes differences between providers so application code stays portable.
- **Prompts**: Template-based prompt construction with support for few-shot examples, dynamic variable insertion, and structured prompt schemas. The `ChatPromptTemplate` class is the standard entry point.
- **Chains**: Sequences of calls that pass outputs from one component as inputs to the next. The legacy `Chain` class has been largely replaced by the **LangChain Expression Language (LCEL)**, which offers a declarative, composable syntax using the pipe operator (`|`).
- **Output Parsers**: Structured extraction from LLM outputs, converting raw text into Pydantic models, JSON, or other typed formats.

### LCEL (LangChain Expression Language)

LCEL is the modern way to build LangChain applications. It provides:

- Declarative composition with the `|` pipe operator
- Streaming support built in across all components
- Async execution with `ainvoke`, `astream`
- Batch processing with automatic concurrency
- Retry and fallback primitives via `with_retry()` and `with_fallbacks()`

Example pattern:
```python
chain = prompt | model | parser
result = chain.invoke({"question": "What is LangChain?"})
```

### Memory and State

LangChain provides several memory abstractions for maintaining conversational context:

- **ConversationBufferMemory**: Stores all messages in a list
- **ConversationSummaryMemory**: Summarizes conversation history to manage token limits
- **ConversationBufferWindowMemory**: Keeps only the last N exchanges

For more sophisticated state management, developers typically graduate to [[langgraph]], which provides explicit graph-based state machines built on top of LangChain primitives.

### Document Processing and RAG

LangChain has extensive support for rag workflows:

- **Document Loaders**: Read from PDFs, HTML, Markdown, APIs, databases, and dozens of other sources
- **Text Splitters**: Chunk documents by character, token, semantic boundaries, or code structure
- **Vector Stores**: Integration with Chroma, Pinecone, Weaviate, FAISS, Milvus, and many others
- **Retrievers**: Combine vector similarity search with hybrid strategies (BM25, parent-document retrieval, contextual compression)

### Tool and Agent Ecosystem

LangChain's tool-use abstractions allow agents to call external functions:

- **Tools**: Wrappers around APIs, calculators, search engines, and custom Python functions
- **Agent Executors**: Pre-built agent types including ReAct, self-ask, plan-and-execute, and structured chat agents
- **Toolkits**: Bundled tool collections for specific domains (SQL, GitHub, Slack, etc.)

For production-grade agents with cycles, persistence, and human-in-the-loop support, [[langgraph]] is now the recommended approach.

## Relationships

- **LangGraph** — [[langgraph]] is LangChain's sister project providing state-machine-based agent orchestration; it reuses LangChain's model and tool abstractions while adding graph-based control flow.
- **LlamaIndex** — [[llamaindex]] is a complementary framework focused on data indexing and retrieval; it can be used alongside LangChain or as an alternative for RAG-heavy workloads.
- **OpenAI Agents SDK** — openai-agents is OpenAI's native agent framework; it provides a more opinionated approach compared to LangChain's modularity.
- **ReAct Pattern** — LangChain's agent executors implement the react pattern for interleaving reasoning and tool use.
- **Prompt Chaining** — LangChain's chain abstraction is a direct implementation of prompt-chaining as an agentic design pattern.

## Related Concepts

[[langgraph]], [[llamaindex]], [[openai-agents-sdk]], [[tool-use-pattern]]

## Sources

- LangChain official documentation
- LangChain GitHub repository
