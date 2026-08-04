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
| `mcp_write` | Create, update, delete — excluding permanent asset deletion | create_asset, update_asset, update_asset_state, create_maintenance_task, update_maintenance_task, create_invite, delete_invite, resend_invite, remove_user, create_workspace, update_workspace, delete_workspace |
| `mcp_destructive` | Permanent, irreversible asset deletion | force_delete_asset |

`mcp_read` is granted by default if no scope is specified.

### Why `mcp_destructive` is separate

`mcp_write` covers deletion, but not deletion of an asset's records. Its delete tools are narrow: `delete_workspace` refuses a workspace that still holds assets or tasks, `remove_user` revokes access without touching the account, `delete_invite` withdraws an invitation that can be issued again, and `update_asset_state` with state `deleted` retires an asset while keeping it and its history intact.

`force_delete_asset` is different — it removes an asset, its maintenance tasks and its entire activity history permanently. So it requires its own scope rather than riding on "create and modify".

**A client must request `mcp_destructive` at registration time.** Scopes are fixed when a client registers, so a client registered before this scope existed cannot obtain it by re-authorizing:

> **Already connected?** You must **re-register** your client, not merely re-authorize it. In most AI applications that means removing the a4b.ai server from your configuration and adding it again. Re-running the authorization flow alone will not offer the new scope.
>
> The tool is listed for every client regardless of scope, so seeing `force_delete_asset` in your tool list does not mean you can use it — calling it without the scope returns `Insufficient scope. Required: mcp_destructive`.

A client asks for its scopes when it registers, in the `scope` field of its `POST /oauth/register` request. All three are advertised in `scopes_supported` on both discovery documents:

```
GET https://a4b.ai/.well-known/oauth-authorization-server
GET https://a4b.ai/.well-known/oauth-protected-resource
```

A client that builds its registration request from `scopes_supported` therefore picks `mcp_destructive` up automatically. One that hard-codes `mcp_read mcp_write` will not, and cannot be granted the scope from a4b.ai's side — the authorization page only offers what the client asked for at registration. If you are writing your own client, request it explicitly at registration; permanent deletion is otherwise available in the web interface.

This is deliberate: an existing integration cannot silently gain the ability to permanently destroy data.

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
