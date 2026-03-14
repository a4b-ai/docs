---
title: What is MCP?
parent: MCP Server
nav_order: 2
description: Learn how the Model Context Protocol enables AI assistants to interact with a4b.ai asset management data.
---

# What is MCP?

The **Model Context Protocol** (MCP) is an open standard that enables AI assistants to interact with external applications and data sources in a secure, standardized way.

## Core Concepts

### Clients and Servers

- **MCP Client**: The AI application (Claude Desktop, ChatGPT, etc.) that connects to servers on behalf of the user
- **MCP Server**: An application (like a4b.ai) that exposes tools and resources for AI clients to use

### Tools

Tools are actions that an AI assistant can invoke. They accept parameters and return results. For example, `list_assets` is a tool that returns a paginated list of assets.

a4b.ai exposes 23 tools covering assets, workspaces, maintenance tasks, users, invites, and QR code generation.

### Resources

Resources are read-only data endpoints that AI clients can browse by URI. They provide context without requiring specific parameters. For example, `a4b://stats` returns organization-wide statistics.

a4b.ai exposes 6 resource templates via `a4b://` URIs.

### Transport

MCP supports multiple transport types. a4b.ai uses **Streamable HTTP** — the server accepts JSON-RPC 2.0 requests over HTTPS at a single endpoint (`POST /mcp`).

## How a4b.ai Uses MCP

```
┌──────────────────┐         HTTPS         ┌──────────────┐
│                  │                       │              │
│  AI Assistant    │ ──── JSON-RPC 2.0 ──→ │  a4b.ai      │
│  (Claude, etc.)  │ ←── Tool responses ── │  MCP Server  │
│                  │                       │              │
└──────────────────┘                       └──────────────┘
        │                                          │
        │  1. OAuth 2.1 authentication             │
        │  2. tools/call → execute actions         │
        │  3. resources/read → browse data         │
        │                                          │
```

The a4b.ai MCP server:

1. **Authenticates** via OAuth 2.1 with PKCE (no client secrets needed)
2. **Authorizes** every request using role-based access policies
3. **Scopes** all data to the authenticated user's organization (multi-tenant)
4. **Logs** every tool call for audit compliance

## JSON-RPC 2.0

Under the hood, MCP uses JSON-RPC 2.0 as its message format. You don't need to write this yourself — your AI client handles it automatically:

```json
// Request: Call a tool
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "list_assets",
    "arguments": { "page": 1 }
  }
}

// Response
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "content": [
      { "type": "text", "text": "{...}" }
    ]
  }
}
```

## Learn More

- [MCP Specification](https://modelcontextprotocol.io/specification/) — Official protocol documentation
- [MCP GitHub](https://github.com/modelcontextprotocol) — Reference implementations and SDKs
- [Quickstart](quickstart) — Connect a4b.ai to your AI assistant
