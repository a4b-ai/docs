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

1. **OAuth scope** — Token must have the required scope (`mcp_read` or `mcp_write`)
2. **Role-based policy** — User permissions are checked for the requested resource
3. **Organization scoping** — All queries scoped to the token's organization
4. **Workspace context** — Workspace-level permissions enforced where applicable

Unauthorized requests are rejected before any data is accessed.

## Audit Logging

All MCP tool calls are logged for compliance and security monitoring. Audit logs are retained for **90 days** and automatically cleaned up.

## Redirect URI Validation

Dynamic client registration validates all redirect URIs. Only HTTPS URLs and localhost loopback addresses (for development) are accepted.

## Connected Apps

Users can view and revoke authorized MCP applications at any time:

- **Settings > Connected Apps** — Lists all authorized applications with scopes and last-used date
- One-click revocation immediately invalidates all tokens for that application

## Privacy

- See our [Privacy Policy](https://a4b.ai/en/privacy-policy) for data handling details
- All data is scoped to the authorized organization
- No data is shared between organizations
