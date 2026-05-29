---
title: Quickstart
parent: MCP Server
nav_order: 1
description: Get the a4b.ai MCP server connected to Claude, ChatGPT, or any MCP client in under 5 minutes.
---

# Quickstart

Get the a4b.ai MCP server connected to your AI assistant in under 5 minutes.

## Prerequisites

- An active [a4b.ai](https://a4b.ai) account
- An MCP-compatible AI client (Claude Desktop, Claude Code, ChatGPT, etc.)

## Server URL

```
https://a4b.ai/mcp
```

The server uses **OAuth 2.1 with PKCE** for authentication. Most MCP clients handle the OAuth flow automatically — you just need to provide the server URL.

## Claude Code

**Option 1** — CLI command:

```bash
claude mcp add --transport http a4b-ai https://a4b.ai/mcp
```

**Option 2** — Project config (`.mcp.json` in your project root):

```json
{
  "mcpServers": {
    "a4b-ai": {
      "type": "http",
      "url": "https://a4b.ai/mcp"
    }
  }
}
```

**Option 3** — Global config (`~/.claude/mcp.json`):

Same format as above. Global config applies to all projects. See the [Claude Code docs](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/tutorials#set-up-model-context-protocol-mcp) for details.

## Claude Desktop

**Option 1** — Connectors UI (recommended):

Go to **Settings → Connectors → Add custom connector**. Enter `https://a4b.ai/mcp` as the server URL and complete OAuth authorization.

**Option 2** — JSON config file:

```json
{
  "mcpServers": {
    "a4b-ai": {
      "command": "npx",
      "args": ["mcp-remote", "https://a4b.ai/mcp"]
    }
  }
}
```

See the [Claude Desktop MCP docs](https://docs.anthropic.com/en/docs/agents-and-tools/remote-mcp#claude-for-desktop) for config file locations and setup details.

## ChatGPT

In ChatGPT settings, add a new MCP connection with server URL `https://a4b.ai/mcp`.

See the [OpenAI MCP documentation](https://platform.openai.com/docs/mcp) for details.

## Custom Clients

For building your own MCP client, use these endpoints:

| Endpoint | URL |
|----------|-----|
| MCP endpoint | `POST https://a4b.ai/mcp` |
| OAuth authorization | `GET https://a4b.ai/oauth/authorize` |
| Token exchange | `POST https://a4b.ai/oauth/token` |
| Client registration | `POST https://a4b.ai/oauth/register` |
| Server metadata | `GET https://a4b.ai/.well-known/oauth-authorization-server` |
| Resource metadata | `GET https://a4b.ai/.well-known/oauth-protected-resource` |

See [Authentication](authentication) for the complete OAuth 2.1 flow.

## First Connection

When you first use an a4b.ai tool, your client will:

1. Open a browser window for you to sign in to a4b.ai
2. Ask you to authorize the MCP client
3. You select which organization to grant access to
4. The client receives an access token and connects

This only happens once — your client stores the token and refreshes it automatically.

## Verifying Your Connection

After setup, verify the connection works by asking your AI assistant:

```
What tools do you have from a4b?
```

The assistant should list the available a4b.ai tools. Then try:

```
Show my organization stats from a4b.ai
```

This calls `get_organization_stats` and confirms your authentication is working.

## Test It Out

Try these prompts with your AI assistant:

| Prompt | What it does |
|--------|-------------|
| "List my assets" | Calls `list_assets` to show all assets in your organization |
| "Show overdue maintenance tasks" | Calls `list_maintenance_tasks` with state filter |
| "What's our organization's asset count?" | Calls `get_organization_stats` |
| "Add a new air compressor to the Main Facility workspace" | Calls `create_asset` with workspace context |
| "Generate QR codes for the first 3 assets" | Calls `generate_asset_qr_codes` |

## Scopes

During authorization, you choose which permissions to grant:

- **`mcp_read`** — Read assets, workspaces, users, maintenance tasks, stats
- **`mcp_write`** — Create, update, and delete resources

`mcp_read` is granted by default. See [Authentication](authentication#scopes) for the full tool-to-scope mapping.

## Next Steps

- [Usage Examples](usage-examples) — Real-world integration scenarios
- [Authentication](authentication) — Deep dive into OAuth 2.1 flow
- [Troubleshooting](troubleshooting) — Common issues and solutions
