# Magpie — USDA Agricultural Data API + MCP Server for AI Agents

Magpie is a free USDA data API and Model Context Protocol (MCP) server for AI agents. It turns hard-to-use USDA agricultural data into clean, agent-ready tools and REST endpoints.

**Free, no API key. 30 MCP tools.**

- **MCP endpoint (streamable HTTP):** https://ag.morningbrief.link/mcp
- **REST base:** https://ag.morningbrief.link — endpoints under `/v1/`
- **Product page:** https://morningbrief.link/ag/
- **Docs:** https://morningbrief.link/docs
- **Hub:** https://morningbrief.link
- **MCP Registry listing:** `link.morningbrief/magpie`

> This repository is a discovery stub. Magpie runs remotely — there is nothing to install or self-host here, and no worker source code is included.

## Datasets

- Grain prices
- Livestock prices
- Dairy prices
- Fertilizer prices
- WASDE projections (supply & demand)
- Export sales (ESR)
- Ag trade flows (GATS)
- NASS statistics, plus crop condition and crop progress
- Weather
- Price-spread analytics

Data is USDA-only. When data is missing, Magpie says so instead of guessing — no data beats misleading data.

## Connect via MCP

Add Magpie to any MCP client with this config:

```json
{
  "mcpServers": {
    "magpie": {
      "url": "https://ag.morningbrief.link/mcp"
    }
  }
}
```

Claude Code CLI:

```bash
claude mcp add --transport http magpie https://ag.morningbrief.link/mcp
```

Any MCP client, and ChatGPT custom connectors, can use the same URL: `https://ag.morningbrief.link/mcp`

## REST examples

- https://ag.morningbrief.link/v1/grain/prices
- https://ag.morningbrief.link/v1/livestock/prices
- https://ag.morningbrief.link/v1/dairy/prices
- https://ag.morningbrief.link/v1/fertilizer/prices
- https://ag.morningbrief.link/v1/wasde/projections

Full documentation: https://morningbrief.link/docs

## Links

- Hub: https://morningbrief.link
- Product page: https://morningbrief.link/ag/
- Docs: https://morningbrief.link/docs
- MCP Registry: search for `link.morningbrief/magpie`

## Cost & access

Free, no API key required.
