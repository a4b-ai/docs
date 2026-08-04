---
title: Troubleshooting
parent: MCP Server
nav_order: 6
description: Common issues and solutions for authentication errors, data errors, and connection problems.
---

# Troubleshooting

Common issues and solutions when using the a4b.ai MCP server with your AI assistant.

## Authentication Errors

### Token expired or invalid

Your AI client shows an authentication error or stops responding to a4b.ai requests.

**Solution**: Most MCP clients refresh tokens automatically. If the error persists:
1. Remove and re-add the a4b.ai MCP server in your client
2. Re-authorize when prompted
3. Check if the application was revoked in **Settings > Connected Apps**

### Insufficient permissions

Your AI client returns "Insufficient scope" when trying to create or update resources.

**Solution**: Grant `mcp_write` as well as `mcp_read`. Sign in again when your client next prompts you, and select both scopes on the authorization page.

Two terms are used below and they mean different things:

- **Re-authorize** — sign in and approve access again, with the server still in your client's configuration. This can change *which of the scopes your client asked for* you approve.
- **Register again** — remove the a4b.ai server from your client's configuration and add it back, so the client registers itself from scratch. This is the only way to change *which scopes your client asks for at all*.

### "Insufficient scope. Required: mcp_destructive"

`force_delete_asset` returns this when called. Note the tool is listed for every client, whether or not the token can use it — so seeing it in your tool list does not mean you have access.

**Solution**: The client has to **register again** — re-authorizing will not work, no matter how many times you do it. Scopes are fixed when a client registers, so a client that registered before `mcp_destructive` existed has no way to be granted it.

Remove the a4b.ai server from your client's configuration entirely, add it back, and select `mcp_destructive` on the authorization page.

If the option still does not appear after registering again, your client does not request that scope, and there is currently no way to enable the tool from a4b.ai's side. Permanent deletion remains available in the web interface, on the asset's edit page, under "Danger zone".

This is intentional. An integration that already has access to your data should not be able to gain the ability to permanently destroy it without being set up again.

### Permanent deletion is refused

`force_delete_asset` returns an error even though your token has `mcp_destructive`.

**Solution**: Check each of these in turn.

- **Confirmation mismatch** — the call must include `confirm_inventory_number` matching the asset's inventory number exactly. Read it with `get_asset` first rather than guessing; a mismatch deletes nothing.
- **Not an administrator** — restricted to organization administrators and administrators of the workspace containing the asset, the same as in the web interface.
- **Wrong organization or workspace** — the asset must be one your token can reach.

If you meant to retire an asset rather than destroy it, use `update_asset_state` with state `deleted` instead. That keeps the asset and its history, and is reversed by setting the state back to `available` or `in_use`.

### Not authorized for a resource

Your AI client returns "Not authorized" for a specific workspace or action.

**Solution**: Your user role doesn't have permission for this action. Contact your organization admin to grant the appropriate role via **Settings > Workspace Members** in a4b.ai.

---

## Data Errors

### Resource not found

Your AI client can't find an asset, workspace, or other resource.

**Causes**:
- The resource ID doesn't exist
- The resource belongs to a different organization
- The resource has been deleted

**Solution**: Ask your AI assistant to list resources first (e.g., "List my assets") to find valid IDs.

### Validation failed

Your AI client returns validation errors when creating or updating resources.

**Solution**: Check the required parameters and accepted formats for the tool. Ask your AI assistant "What parameters does [tool name] accept?" to see the tool's schema.

---

## Connection Issues

### Client doesn't show a4b.ai tools

1. Verify the server URL is exactly `https://a4b.ai/mcp`
2. Restart your MCP client after configuration changes
3. Check that your config file is valid JSON
4. Check client logs for connection errors
5. If the tools appear but a call fails with "Insufficient scope", that is a scope issue rather than a connection one — see [Insufficient permissions](#insufficient-permissions) or, for permanent deletion, ["Insufficient scope. Required: mcp_destructive"](#insufficient-scope-required-mcp_destructive)

### OAuth popup doesn't appear

1. Check that your browser isn't blocking popups from the MCP client
2. Ensure you have an active a4b.ai account

### Token refresh fails

1. Refresh tokens are single-use — each refresh returns a new one
2. If your client used an old refresh token, re-authenticate through the full OAuth flow
3. Check if the application was revoked in **Settings > Connected Apps**

---

## Plan Limits & Authorization

### Plan limit reached

Your AI client returns an error when creating assets, workspaces, or inviting users.

**Solution**: Your organization has reached the capacity limit for the current plan. Check your usage with "Show my organization stats" — if you're at capacity, upgrade your plan or remove unused resources.

### PKCE failure (`invalid_grant`)

Your MCP client fails during the OAuth flow with `invalid_grant` or similar errors.

**Causes**:
- The authorization code expired (codes are single-use and short-lived)
- The PKCE code verifier doesn't match the code challenge
- The redirect URI doesn't match what was registered

**Solution**: Remove and re-add the MCP server to start a fresh OAuth flow. Ensure your client supports PKCE (most modern MCP clients do).

### Rate limited (429)

Your AI client receives "Too Many Requests" responses.

**Solution**: The MCP server rate-limits client registrations per IP. Wait a moment and try again.

---

## Still stuck?

Ask your AI assistant "What's my current user?" — this confirms your connection is working. If that fails, remove and re-add the server configuration.

Contact us at [support@a4b.ai](mailto:support@a4b.ai) for further assistance.
