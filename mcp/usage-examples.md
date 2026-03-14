---
title: Usage Examples
parent: MCP Server
nav_order: 4
description: Real-world scenarios showing how AI assistants use a4b.ai MCP tools for asset management.
---

# Usage Examples

Real-world scenarios showing how AI assistants use a4b.ai MCP tools. Just type a natural language prompt — the AI handles tool calls automatically.

{: .note }
> **Tip**: The more context you give your AI assistant, the better the results. Include workspace names, asset details, and dates in your prompts.

---

## Example 1: Browse Your Assets

> Show me all assets in the Engineering workspace

The AI finds the Engineering workspace and lists all assets in it, showing name, inventory number, state, and assignment.

**Example response**: "The Engineering workspace has 42 assets. Here are the highlights: 20 are in use, 12 available, 5 stored, and 3 in maintenance. The most recently added is a MacBook Pro 16-inch (#5-000099), currently available."

---

## Example 2: Create an Asset and Schedule Maintenance

> Add a new MacBook Pro to Engineering and schedule a setup task for next week

The AI creates the asset in the Engineering workspace, then schedules a maintenance task linked to it.

**Example response**: "Done! Created MacBook Pro 16-inch (inventory #5-000099) in Engineering and scheduled 'Initial setup and software installation' for March 20th."

---

## Example 3: Find Overdue Maintenance

> Are there any overdue maintenance tasks?

The AI checks all maintenance tasks with an overdue status and summarizes what needs attention.

**Example response**: "You have 3 overdue maintenance tasks:
1. **Quarterly hardware inspection** for Lenovo ThinkPad X1 — due March 1st (14 days overdue)
2. **Battery replacement** for Dell Monitor — due February 28th
3. **Firmware update** for Network Switch — due February 15th"

---

## Example 4: Audit Asset Change History

> Show me all changes to asset #42 in the last month

The AI searches the change history for that asset and presents a timeline of modifications.

**Example response**: "Asset #42 (Lenovo ThinkPad X1) had 5 changes in the last month:
- Feb 28: State changed from 'available' to 'in_use' by John Doe
- Feb 25: Assigned to John Doe (was unassigned)
- Feb 20: Description updated
- Feb 15: Serial number added
- Feb 13: Asset created by Admin"

---

## Example 5: Organization Dashboard Overview

> Give me an overview of our organization's assets and maintenance status

The AI retrieves organization-wide statistics and presents a summary.

**Example response**: "Here's your organization overview for Acme Corporation (Premium plan):

**Assets** (42 of 500 capacity): 20 in use, 12 available, 5 stored, 3 in maintenance, 2 deleted.

**Maintenance**: 15 total tasks — 8 scheduled, 2 in progress, 4 completed, 1 overdue. 3 tasks due this week, 1 due today.

**Plan usage**: Assets 42/500 (8%), Users 8/50 (16%), Workspaces 3/5 (60%)."

---

## Example 6: Invite Team Members

> Invite alice@company.com and bob@company.com to the Engineering workspace

The AI sends invitations to both email addresses for the Engineering workspace.

**Example response**: "Invitations sent to alice@company.com and bob@company.com for the Engineering workspace. They'll receive emails with instructions to join."

---

## Example 7: Manage Workspaces

> Create a new workspace called QA Lab

The AI creates a workspace with the given name in your organization.

**Example response**: "Created workspace 'QA Lab'. It's ready to use — you can start adding assets or inviting team members to it."

---

## Example 8: Generate QR Code Labels

> Generate QR codes for assets #1, #2, and #3

The AI fetches the assets by ID and generates downloadable QR code labels for each one.

**Example response**: "Generated QR codes for 3 assets:
1. **MacBook Pro 16-inch** (#5-000001) — QR links to asset detail page
2. **Dell Monitor 27-inch** (#5-000002) — QR links to asset detail page
3. **Network Switch** (#5-000003) — QR links to asset detail page

Each QR code encodes the asset's direct URL for quick scanning."

---

## Example 9: Manage Team Access

> List all users in the Engineering workspace and remove the ones who haven't logged in since January

The AI lists workspace members, identifies inactive ones, and removes them after confirmation.

**Example response**: "The Engineering workspace has 12 members. 3 haven't logged in since January:
- charlie@company.com (last active Dec 15)
- dana@company.com (last active Nov 30)
- eve@company.com (last active Jan 2)

Removed all 3 from the Engineering workspace. They still have access to other workspaces in the organization."
