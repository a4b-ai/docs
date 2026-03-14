---
title: MCP Server
nav_order: 1
has_children: true
description: Connect AI assistants to your asset management data via the Model Context Protocol — 23 tools, OAuth 2.1 security.
---

# MCP Server

The a4b.ai MCP Server provides AI assistants with secure, real-time access to your asset management data through the [Model Context Protocol](https://modelcontextprotocol.io/).

**Server URL**: `https://a4b.ai/mcp`
{: .fs-5 }

## Key Features

| Feature | Details |
|---------|---------|
| **23 Tools** | Assets, workspaces, maintenance tasks, users, invites, QR codes |
| **6 Resource Templates** | Browsable read-only data via `a4b://` URIs |
| **OAuth 2.1 + PKCE** | Industry-standard security, no client secrets needed |
| **Multi-Tenant** | Organization-scoped access tokens |
| **Audit Logging** | Every tool call logged for compliance (90-day retention) |
| **Dynamic Client Registration** | [MCP spec](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization#dynamic-client-registration)-compliant automated onboarding |

## Who Is This For?

- **AI developers** integrating Claude, ChatGPT, or other MCP-compatible tools with asset management
- **Organizations** wanting AI-assisted asset tracking and maintenance scheduling
- **System integrators** building MCP-based automation workflows

## Supported Platforms

Works with any MCP-compatible client:

- [Claude Desktop](quickstart#claude-desktop)
- [Claude Code](quickstart#claude-code)
- [ChatGPT](quickstart#chatgpt)
- Any MCP-compatible client

## Quick Links

- [Quickstart](quickstart) — Get connected in under 5 minutes
- [Authentication](authentication) — OAuth 2.1 flow details
- [Usage Examples](usage-examples) — Real-world integration scenarios
- [Security & Compliance](security) — Data isolation and audit logging
- [Changelog](changelog) — Version history and release notes
