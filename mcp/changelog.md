---
title: Changelog
parent: MCP Server
nav_order: 7
description: Version history and release notes for the a4b.ai MCP server.
---

# Changelog

All notable changes to the a4b.ai MCP server.

## v1.2.0 — August 2026

### Features

- **`force_delete_asset`** — permanently deletes one asset together with its maintenance tasks and activity history. Administrators only, and irreversible.
- **`mcp_destructive` scope**, required by that tool and by nothing else. It is deliberately not part of `mcp_write`.

To retire an asset instead, use `update_asset_state` with state `deleted`. That keeps the asset and its history, and is reversed by setting the state back to `available` or `in_use`.

### Action required for existing clients

`force_delete_asset` is listed for every client, but calling it without the new scope returns `Insufficient scope. Required: mcp_destructive`. To obtain the scope your client must be **registered again** — re-authorizing is not sufficient, because scopes are fixed at registration. Remove the a4b.ai server from your client configuration and add it back. See [Authentication](authentication#why-mcp_destructive-is-separate).

Clients that do not need permanent deletion require no action.

### Fixed

- **`create_asset` no longer fails when two assets are created at the same moment.** Previously a collision on the server-generated inventory number surfaced as `Validation failed: Inventory number has already been taken`, or occasionally as an internal error. It now re-derives the number and succeeds.
- **Inventory numbers are never reissued.** A number belonging to a permanently deleted asset is never handed to a new one, so gaps in the sequence are expected and a number you cached will never come to refer to a different asset.

### Safeguards

Each call must confirm the asset's exact inventory number, deletes at most one asset, and is recorded in an audit entry retained indefinitely — the only remaining record once the asset is gone. See [Security](security#permanent-deletion).

## v1.1.0 — June 2026

- A4B CMMS is now listed in the ChatGPT app directory. ChatGPT users can connect directly without manual connector configuration: [chatgpt.com/apps/a4b-cmms](https://chatgpt.com/apps/a4b-cmms/asdk_app_69fce78ea9a48191af1a9f23dbe06314).
- Manual connector setup (`https://a4b.ai/mcp`) remains available for users who prefer it.

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
