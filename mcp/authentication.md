---
title: Authentication
parent: MCP Server
nav_order: 3
description: OAuth 2.1 with PKCE authentication flow, scopes, token lifecycle, and metadata discovery.
---

# Authentication

The a4b.ai MCP server uses **OAuth 2.1 with PKCE** for authentication. MCP clients handle the OAuth flow automatically — you just provide the server URL and authorize when prompted.

{: .note }
> Most users don't need this page. See [Quickstart](quickstart) to get connected in under 5 minutes.

## How It Works

1. **Client registration** — Your MCP client registers itself automatically via [Dynamic Client Registration](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization#dynamic-client-registration)
2. **Authorization** — You sign in to a4b.ai, select an organization, and grant access
3. **Token exchange** — The client receives an access token (valid 2 hours) and a refresh token
4. **API calls** — The client uses the token for all MCP requests, refreshing automatically when needed

## Scopes

| Scope | Access | Tools |
|-------|--------|-------|
| `mcp_read` | Read-only | list_assets, get_asset, list_workspaces, list_maintenance_tasks, get_maintenance_task, search_asset_history, get_organization_stats, get_current_user, list_invites, list_users, generate_asset_qr_codes |
| `mcp_write` | Create, update, delete | create_asset, update_asset, update_asset_state, create_maintenance_task, update_maintenance_task, create_invite, delete_invite, resend_invite, remove_user, create_workspace, update_workspace, delete_workspace |

`mcp_read` is granted by default if no scope is specified.

## Endpoints

| Endpoint | Purpose |
|----------|---------|
| `POST /oauth/register` | Dynamic client registration ([MCP spec](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization#dynamic-client-registration)) |
| `GET /oauth/authorize` | Authorization request |
| `POST /oauth/token` | Token exchange and refresh |
| `POST /oauth/revoke` | Token revocation |

## Token Lifecycle

| Property | Value |
|----------|-------|
| Access token expiry | 2 hours |
| Refresh token expiry | Never (until revoked) |
| Refresh token rotation | New token on each use, old revoked |
| Revocation | Via `POST /oauth/revoke` or **Settings > Connected Apps** in a4b.ai |

## Organization Scoping

Access tokens are scoped to a **single organization**. During authorization, you select which organization to grant access to. Users in multiple organizations need separate tokens for each.

## Metadata Discovery

MCP clients auto-discover OAuth configuration via standard endpoints:

| URL | Standard |
|-----|----------|
| `https://a4b.ai/.well-known/oauth-authorization-server` | RFC 8414 |
| `https://a4b.ai/.well-known/oauth-protected-resource` | RFC 9728 |

## Security Details

For data isolation, authorization layers, and audit logging details, see [Security & Compliance](security).
