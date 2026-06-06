# Copilot CLI Setup Guide

This organisation uses [GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli) with a Notion integration so that Copilot can read specs and documentation directly from our Notion workspace.

## Prerequisites

- An active [GitHub Copilot subscription](https://github.com/features/copilot/plans)
- [Copilot CLI installed](https://docs.github.com/copilot/how-tos/use-copilot-agents/use-copilot-cli)
- Node.js (v18+) installed (required for the Notion MCP server)

## Setting up Notion access

### 1. Get your Notion Integration Token

Ask a workspace admin to invite you to the **Copilot CLI** integration, or create your own:

1. Go to [https://www.notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click **"+ New integration"**
3. Name: `Copilot CLI`
4. Workspace: Select the **Kynd** workspace
5. Capabilities: Ensure **Read content** is enabled
6. Click **Submit** and copy the token (starts with `ntn_`)

### 2. Share Notion pages with the integration

For any page or database you want Copilot to read:

1. Open the page in Notion
2. Click the **three-dot menu** (top-right) then **Connections** then **Add connection**
3. Select **Copilot CLI**

> **Tip:** Share a top-level page and all child pages will be accessible too.

### 3. Add the token to your environment

Add this line to your shell profile (`~/.zshrc`, `~/.bashrc`, or `~/.profile`):

```bash
export NOTION_TOKEN="ntn_your_token_here"
```

Then reload your shell:

```bash
source ~/.zshrc  # or ~/.bashrc
```

### 4. Verify it works

Launch Copilot CLI in any Kynd-Software repo and run:

```
/mcp
```

You should see `notion` listed as a configured server. Try asking:

> "List my recent Notion pages"

## How the configuration works

The MCP (Model Context Protocol) config lives in this repo at `.github/copilot-mcp.json`. It automatically applies to **all repositories** in the Kynd-Software organisation. You do not need to configure anything per-repo.

The config references `NOTION_TOKEN` from your local environment. Your token is never committed to source control.

## Issue Templates

We have an org-wide issue template ("Task Ticket") that provides a consistent structure for tickets. When creating a new issue in any Kynd-Software repo:

1. Go to **Issues** then **New Issue**
2. Select the **"Task Ticket"** template
3. Fill in the sections (Summary, Context, Acceptance Criteria, etc.)
4. Link relevant Notion docs in the **Context / Why** section

## Workflows

### Creating tickets from Notion specs

You can ask Copilot CLI to read a Notion page and create a GitHub issue:

```
Read the spec at https://notion.so/your-page-id and create a GitHub issue for it
```

### Referencing Notion in tickets

Include Notion links in your issues. Copilot can fetch the content when working on the ticket:

```markdown
## Context / Why
See full spec: https://notion.so/kynd/Feature-Spec-abc123
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `notion` not showing in `/mcp` | Ensure `NOTION_TOKEN` is exported and restart your terminal |
| "Could not read page" errors | Check the page is shared with the Copilot CLI integration in Notion |
| MCP server fails to start | Run `npx @notionhq/notion-mcp-server` manually to see errors. Ensure Node.js v18+ is installed |

## Questions?

Reach out in the team channel or open a discussion in this repo.
