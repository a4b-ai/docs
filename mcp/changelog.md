---
title: Changelog
parent: MCP Server
nav_order: 7
description: Version history and release notes for the a4b.ai MCP server.
---

# Changelog

All notable changes to the a4b.ai MCP server.

## v1.0.0 — March 2026

Initial public release of the a4b.ai MCP server.

### Features

- **23 Tools** for managing assets, workspaces, maintenance tasks, users, and invites
- **6 Resource Templates** providing browsable read-only data via `a4b://` URIs
- **OAuth 2.1 + PKCE** authentication with dynamic client registration ([MCP spec](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization#dynamic-client-registration))
- **Multi-tenant** organization-scoped access tokens
- **Audit logging** with 90-day retention for compliance
- **Safety annotations** on all tools (`readOnlyHint`, `destructiveHint`, `idempotentHint`)
- **QR code generation** for asset labels
- **Metadata discovery** via RFC 8414 and RFC 9728 endpoints

### Supported Platforms

- Claude Desktop
- Claude Code
- ChatGPT
- Any MCP-compatible client
