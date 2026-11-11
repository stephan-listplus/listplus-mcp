# Installing the ListPlus MCP server (for AI agents)

ListPlus is a hosted, remote MCP server. There is nothing to clone, build or
run locally — setup is one entry in the client's MCP configuration.

## 1. Get an agent key

Ask the user for their ListPlus agent key. It starts with `lpk_` and is
created in ListPlus under **Agents**: https://listplus.ai/app/agents
(a ListPlus account is needed; the Basic plan costs nothing).

Never invent a key. Without one, add the server without the `headers` block:
the data checks (`check_records`), market sizing — company counts (`count_companies`),
`search_options` and price estimates still work.

## 2. Add the server

Transport: **Streamable HTTP**. URL: `https://listplus.ai/mcp`.

Cline (`cline_mcp_settings.json`):

```json
{
  "mcpServers": {
    "listplus": {
      "type": "streamableHttp",
      "url": "https://listplus.ai/mcp",
      "headers": {
        "Authorization": "Bearer lpk_YOUR_KEY"
      },
      "disabled": false
    }
  }
}
```

Other clients: the same URL with the header
`Authorization: Bearer lpk_YOUR_KEY`. A client without a header field can
use `https://listplus.ai/mcp/lpk_YOUR_KEY` as the address.

## 3. Verify

Call the tool `what_can_you_do`. It lists every action with its price and,
with a key, the workspace's balance and day budget. Then try
`check_records` with two or three sample contact records — it costs no
credits and stores nothing.

## Notes for the agent

- Paid tools say their price at the start of their description. Before a
  paid step, tell the user the price and wait for their agreement;
  `enrich_records`, `search_people` and `load_companies` take
  `estimate: true` for an exact figure first.
- A `401` means the key is missing, wrong or revoked — ask the user for a
  current key instead of retrying.
