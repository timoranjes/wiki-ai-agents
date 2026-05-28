---
title: Model Context Protocol (MCP)
created: 2026-07-11
updated: '2026-05-28'
type: entity
tags:
- anthropic
- tool-use
- open-source
sources: [https://modelcontextprotocol.io/introduction, https://modelcontextprotocol.io/specification/2025-06-18, https://www.anthropic.com/news/model-context-protocol]
confidence: high
---

# Model Context Protocol (MCP)

## Overview

Model Context Protocol (MCP) is an open protocol for connecting AI applications to external tools, data sources, and reusable prompts through a common interface. It was introduced by Anthropic and quickly became one of the most important standardization efforts in the AI agent ecosystem.

The core idea is to reduce integration sprawl. Instead of every agent framework and IDE building custom connectors for every database, filesystem, SaaS API, or internal service, MCP defines a common way for hosts and servers to communicate. That gives tool builders one standard integration target and gives agent builders a shared compatibility layer.

## Key Facts

- Introduced by [[anthropic]]
- Standardizes communication between AI hosts and external tool servers
- Supports local and remote transports
- Exposes three major primitives: tools, resources, and prompts
- Adopted across agent products, IDEs, and developer tooling
- JSON-RPC-based protocol with strongly typed message formats
- Open specification maintained with community contribution processes
- Backed by a growing ecosystem of open-source and commercial server implementations

## Architecture

### Host, client, and server model

An MCP-enabled application acts as the host and connects to MCP servers through a protocol client. The server exposes capabilities to the host, while the model decides when and how those capabilities should be used inside an agent loop.

The three-party architecture separates concerns: the host manages the user-facing application and model interactions, the client handles protocol-level communication, and the server implements specific tool, resource, or prompt capabilities. This decoupling means any MCP-compatible host can discover and use any MCP-compatible server without custom integration work.

### Core primitives

- **Tools** — executable actions the model can invoke with structured input and output schemas
- **Resources** — readable context objects such as files, rows, or documents that the model can reference but not modify
- **Prompts** — reusable prompt templates supplied by the server that the host can populate with arguments

### Transport layer

MCP is designed to work across local and remote deployment patterns. Local developer workflows often use stdio transport, where the server runs as a subprocess and communicates over standard input/output. Networked services can expose MCP-compatible endpoints using the Streamable HTTP transport, which supports remote servers behind authentication and load balancers.

### Discovery and capability negotiation

When a host connects to an MCP server, they perform an initialization handshake that exchanges capabilities, protocol versions, and supported features. This means hosts can discover what tools, resources, and prompts a server provides at connection time, rather than requiring static configuration. Servers can also send notifications to inform hosts about newly available capabilities or changes to existing ones.

### Security and governance

MCP servers run in the host's security context, which means the host is responsible for authenticating users, authorizing tool access, and auditing tool invocations. Servers can implement their own authentication for remote deployments, but the protocol itself does not mandate a specific security model. This design gives hosts flexibility but also places the burden of governance on the application layer.

For enterprise deployments, this means MCP should be combined with existing identity and access management infrastructure. Tool-level permissions, rate limiting, and invocation logging are typically implemented at the host or server level rather than within the protocol itself.

## Framework Support

MCP has gained broad support across the agent ecosystem. [[langchain]] provides MCP tool integration through its MCP client wrapper, allowing LangChain agents to discover and invoke MCP tools as native LangChain tools. [[langgraph]] can use MCP servers as external tool nodes within graph-based agent workflows.

[[crewai]] supports MCP through its tool-abstraction layer, enabling CrewAI agents to connect to MCP servers and treat exposed tools as first-class crew member capabilities. [[autogen]] has MCP integration for multi-agent scenarios where individual agents can access shared MCP tool servers.

[[openai-agents-sdk]] includes MCP as a first-class tool source, allowing agents to load tools from MCP servers alongside natively defined Python functions. The [[google-adk]] also supports MCP as an external tool provider within its agent orchestration model.

IDE integration is a major adoption vector. Major code editors and development environments implement MCP hosts, giving coding agents access to filesystem, git, linter, and build-tool servers through a standardized protocol.

## When to Use

MCP is the right choice when:

- You need to expose the same set of tools to multiple agent frameworks or IDEs without writing custom integrations for each
- You are building a tool provider and want your capabilities discoverable by any MCP-compatible host
- You want to separate tool infrastructure from agent logic so each can evolve independently
- You need a protocol that works equally well for local developer tools and remote enterprise services
- You are standardizing tool access across an organization with multiple AI applications
- You want to enable third-party developers to extend your agent with custom tool servers

MCP is less appropriate when you need deep, framework-specific optimizations that the generic protocol cannot express, or when your tools are tightly coupled to a single agent architecture and unlikely to be reused elsewhere.

## Why It Matters for Agents

MCP is strategically important because it separates model capability from integration capability. In practice, many agent systems succeed or fail based on tool access, not just base-model intelligence. A strong protocol ecosystem raises the ceiling for all compatible agent products.

It also changes the competitive shape of the market. Instead of proprietary one-off integrations, vendors can compete on quality of servers, workflow ergonomics, authentication, governance, and ecosystem depth. That is why MCP has become relevant well beyond Anthropic's own products.

The protocol approach mirrors what happened with databases and APIs in earlier computing eras: standardization creates network effects that benefit the entire ecosystem. MCP's trajectory suggests it may become the default tool connectivity layer for AI applications.

## Ecosystem Role

MCP sits at the intersection of agent frameworks, developer environments, and enterprise tool layers. It is closely related to [[function-calling]], but works at a broader systems level: [[function-calling]] is the model-side invocation pattern, while MCP is a protocol for exposing capabilities consistently across tools and hosts.

Within the [[tool-use-pattern]], MCP provides the infrastructure layer that standardizes how tools are discovered, described, and invoked. This complements the cognitive patterns that decide which tools to use and when. For [[coding-agents]] specifically, MCP servers can expose file editing, git operations, test execution, and code review capabilities in a framework-agnostic way.

The protocol also enables a marketplace dynamic where tool server developers can build once and reach multiple agent platforms. This is similar to how browser extensions or IDE plugins work, but applied to the agent tool layer.

## Related Concepts

[[anthropic]], [[function-calling]], [[tool-use-pattern]], [[openai-agents-sdk]], [[langchain]], [[coding-agents]], [[react-pattern]]

## Sources

- Anthropic MCP launch and documentation
- modelcontextprotocol.io
- Framework-specific MCP integration documentation
