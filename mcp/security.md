---
title: Security
parent: MCP Server
nav_order: 5
description: Data isolation, authorization layers, audit logging, and privacy controls in the a4b.ai MCP server.
---

# Security & Compliance

The a4b.ai MCP server is built with security-first principles: OAuth 2.1 with mandatory PKCE, multi-tenant data isolation, role-based authorization, and comprehensive audit logging.

## Data Isolation

Access tokens are scoped to a single organization. All data queries are filtered by organization — cross-organization access is not possible. Users in multiple organizations must authorize separately for each.

## Authorization

Every tool call goes through multiple authorization layers:

1. **OAuth scope** — Token must have the required scope (`mcp_read`, `mcp_write`, or `mcp_destructive`)
2. **Role-based policy** — User permissions are checked for the requested resource
3. **Organization scoping** — All queries scoped to the token's organization
4. **Workspace context** — Workspace-level permissions enforced where applicable

Unauthorized requests are rejected before any data is accessed.

## Permanent Deletion

**To retire an asset, use `update_asset_state` with state `deleted`.** That keeps the asset and its full history, and can be reversed by setting the state back to `available` or `in_use`. It is what "delete this asset" almost always means, and it is the only deletion most integrations should ever perform.

One tool goes further. `force_delete_asset` removes an asset along with its maintenance tasks and its entire activity history, and **it cannot be undone** — there is no recovery path. Two constraints apply to it on top of the authorization layers above:

- **One asset per call.** The tool takes a single asset id and there is no bulk equivalent on this interface. Note this bounds each *call*, not each instruction — an assistant told to "clear out this workspace" can still iterate.
- **Explicit confirmation.** The call must include `confirm_inventory_number` matching the asset's inventory number exactly. **Read the value with `get_asset` first — never construct or guess it.** Numbers a4b.ai generated look like `434-000001`, but an administrator can set any value, so there is no format to infer from. Pass it exactly as returned — do not trim, pad or normalise it. A mismatch deletes nothing. Because the number has to be read first, an assistant cannot delete an asset it has not actually looked at.

It also requires its own OAuth scope, `mcp_destructive`, and is restricted to organization administrators and to administrators of the workspace the asset belongs to. See [Authentication](authentication#why-mcp_destructive-is-separate) for why the scope is separate and what that means for a client that is already connected.

A repeat call is **not** a no-op: once the asset is gone, calling again returns `Asset not found`. Clients that retry automatically on error will surface a second, confusing failure after a deletion that in fact succeeded.

The success response reports how many maintenance tasks and activity-log entries were removed alongside the asset. That is the only confirmation of what went with it.

The confirmation parameter is a server-side guard, not a substitute for asking. An assistant acting on a person's behalf should confirm the specific asset with them in conversation before calling this tool.

Every permanent deletion leaves an audit entry recording who performed it, the organization and workspace, the asset's inventory number and name, the number of maintenance tasks and activity-log entries removed with it, and when. The entry is written before the asset is removed and within the same transaction, so a deletion cannot succeed without one.

**These entries are not pruned on a schedule**, unlike the tool-call logs described below — once the asset is gone they are the only remaining record that it ever existed. Two data-protection cases do reach them: if the acting user later deletes their account, the entry survives with the actor's identity cleared; and if the organization itself is deleted, its entries go with it.

There is no self-service view or export for these entries, and no tool returns them. If you need to establish who deleted an asset and when, contact [support@a4b.ai](mailto:support@a4b.ai). Do not design a recovery process that depends on reading them yourself.

## Audit Logging

All MCP tool calls are logged for compliance and security monitoring. These tool-call logs are retained for **90 days** and automatically cleaned up.

Permanent-deletion audit entries are separate and are **not** subject to this schedule — see [Permanent Deletion](#permanent-deletion).

## Redirect URI Validation

Dynamic client registration validates all redirect URIs. Only HTTPS URLs and localhost loopback addresses (for development) are accepted.

## Connected Apps

Users can view and revoke authorized MCP applications at any time:

- **Settings > Connected Apps** — Lists all authorized applications with scopes and last-used date
- One-click revocation immediately invalidates all tokens for that application

### Withdrawing a scope

A token carries the scopes it was issued with. Re-authorizing an integration with a narrower selection issues a *new*, narrower token — it does not shorten the reach of one already issued, which keeps working until it expires.

So to take a capability away from an integration that already holds it — `mcp_destructive` in particular — **revoke it in Settings > Connected Apps** rather than re-authorizing. Revocation takes effect immediately; waiting for expiry leaves the capability live for up to the access token's lifetime.

## Privacy

- See our [Privacy Policy](https://a4b.ai/en/privacy-policy) for data handling details
- All data is scoped to the authorized organization
- No data is shared between organizations
