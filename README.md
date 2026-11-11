# ListPlus MCP server

Remote [Model Context Protocol](https://modelcontextprotocol.io) server of
[ListPlus](https://listplus.ai) — B2B contact data your AI agent can trust.

- **Check records** — 100+ data-quality checks (typos in e-mail domains, role
  mailboxes, duplicates, placeholders, phone codes that do not fit the
  country), each finding with a proposed fix. Nothing is stored.
- **Find people and companies** — look up one person, search by role,
  seniority, industry, place and company size, or size a market — count the companies matching a country,
  industry, NAICS code and size before loading a record.
- **Fill missing fields** — work e-mail, mobile, job title, LinkedIn profile
  and company data from several providers. The agent gets an estimate and a
  maximum before a paid step runs.
- **Work in lists** — add, update, tag and score records in the lists shared
  with the connection, add columns and rules, hand records to a pipeline
  (CRM, sequencer, Slack, webhook).

This repository holds the server's public metadata and setup instructions.
The server itself is hosted by ListPlus; there is nothing to install or run.

| | |
|---|---|
| Server URL | `https://listplus.ai/mcp` (streamable HTTP) |
| MCP Registry | `ai.listplus/listplus` ([`server.json`](server.json)) |
| Tools | 26 |
| Authentication | OAuth (Claude, ChatGPT, Claude Code) or an agent key `lpk_…` |
| REST API | `https://listplus.ai/a` · [OpenAPI](https://listplus.ai/a/openapi.json) |
| Documentation | https://listplus.ai/en/ai-agents |
| Pricing | https://listplus.ai/en/pricing |

Data checks, market sizing (how many companies match a filter) and price estimates work without an account and
cost no credits. The person search, lists and enrichment need a ListPlus
account; paid steps use the workspace's credits, within a day budget per
connection.

## Connect

### Claude (web and desktop)

Settings → Connectors → Add custom connector, name `ListPlus`, URL
`https://listplus.ai/mcp`. Claude asks you to sign in to ListPlus when a tool
needs it.

### ChatGPT

Add `https://listplus.ai/mcp` as a custom MCP connector in ChatGPT's
connector settings (https://chatgpt.com/plugins) and sign in when asked.

### Claude Code

```bash
claude mcp add --transport http listplus https://listplus.ai/mcp --scope user
```

Then `/mcp` in Claude Code to sign in. With a key instead:

```bash
claude mcp add --transport http listplus https://listplus.ai/mcp \
  --header "Authorization: Bearer lpk_YOUR_KEY" --scope user
```

### Cursor

`~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):

```json
{
  "mcpServers": {
    "listplus": {
      "url": "https://listplus.ai/mcp",
      "headers": { "Authorization": "Bearer lpk_YOUR_KEY" }
    }
  }
}
```

This repository is also a Cursor plugin (`.cursor-plugin/plugin.json`,
`mcp.json`): installed from the Cursor Marketplace, it asks for the key once.

### Cline

`cline_mcp_settings.json` (details for agents: [`llms-install.md`](llms-install.md)):

```json
{
  "mcpServers": {
    "listplus": {
      "type": "streamableHttp",
      "url": "https://listplus.ai/mcp",
      "headers": { "Authorization": "Bearer lpk_YOUR_KEY" }
    }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "listplus": {
      "type": "http",
      "url": "https://listplus.ai/mcp",
      "headers": { "Authorization": "Bearer lpk_YOUR_KEY" }
    }
  }
}
```

### Any other MCP client, n8n, code

Use `https://listplus.ai/mcp` with the header
`Authorization: Bearer lpk_YOUR_KEY` (or `X-API-Key: lpk_YOUR_KEY`). A client
without a header field can use the key in the address:
`https://listplus.ai/mcp/lpk_YOUR_KEY`.

Create a key in ListPlus under **Agents** (https://listplus.ai/app/agents).
An admin decides which lists a connection may read or edit and how many
credits it may spend per day.

## Tools

| Group | Tools |
|---|---|
| Start | `what_can_you_do` |
| Check | `check_records`, `verify_email` |
| Find | `find_person`, `search_options`, `count_companies`, `load_companies`, `search_people` |
| Enrich | `enrich_records`, `get_job` |
| Lists | `describe_list`, `query_records`, `add_records`, `update_records`, `mark_records`, `check_list`, `fix_list`, `derive_column`, `copy_records`, `send_to_pipeline` |
| Set up | `describe_setup`, `create_list`, `add_column`, `remove_column`, `create_trigger`, `update_trigger` |

Every tool's description starts with its price. Records are never deleted
through an agent.

## Example prompts

- "Check these 20 contacts for errors before I import them into HubSpot."
- "How many software companies with 50–200 employees are there in Germany,
  by state?"
- "What would it cost to get work e-mails for these 10 people?"
- "Find 25 RevOps managers at SaaS companies in the US and put them into a
  new list."

## Links

- Website: https://listplus.ai
- Privacy: https://listplus.ai/en/privacy · Terms: https://listplus.ai/en/terms
- Security and data handling: https://listplus.ai/en/security
- Support: support@listplus.ai

ListPlus is built by salesperformer GmbH, Germany, and hosted in the EU.
