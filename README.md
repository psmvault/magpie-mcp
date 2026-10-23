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

## All 30 MCP tools

**Grain prices (USDA MARS daily reports)**

| Tool | What it does |
|------|--------------|
| `get_grain_price` | Latest cash grain price for one commodity in one state (e.g. Illinois corn) |
| `get_grain_history` | Daily cash price history for one grain commodity in one state |
| `list_grain_products` | Lists tracked grain commodities: corn, soybeans, wheat, sorghum, oats, barley, rice |

**Livestock prices (USDA MARS direct-trade and auction reports)**

| Tool | What it does |
|------|--------------|
| `get_livestock_price` | Latest price for one livestock category in one region |
| `get_livestock_history` | Price history for one livestock category in one region |
| `list_livestock_products` | Lists tracked categories: fed cattle, feeder cattle, cows, hogs, sheep |

**Dairy prices (USDA MARS weekly reports)**

| Tool | What it does |
|------|--------------|
| `get_dairy_price` | Latest price for one dairy product (butter price is deliberately withheld — USDA publishes no US base price for butter) |
| `get_dairy_history` | Weekly price history for one dairy product |
| `list_dairy_products` | Lists tracked dairy products: cheese, butter, dry whey, fluid milk |

**Fertilizer prices (USDA MARS twice-monthly production cost reports)**

| Tool | What it does |
|------|--------------|
| `get_fertilizer_price` | Latest price for one fertilizer product (Illinois-led regional coverage) |
| `get_fertilizer_history` | Price history across twice-monthly report periods |
| `list_fertilizer_products` | Lists tracked products: anhydrous ammonia, urea, DAP, MAP, potash, UAN 28/32, ATS, farm diesel, biodiesel |

**WASDE supply & demand (USDA FAS PSD)**

| Tool | What it does |
|------|--------------|
| `get_wasde_projection` | Latest WASDE balance sheet for one commodity: stocks, production, trade, consumption |
| `get_wasde_history` | How one projection figure (e.g. ending stocks) was revised across monthly reports |
| `list_wasde_commodities` | Lists tracked commodities: corn, wheat, soybeans, soybean meal, soybean oil, rice, barley, cotton |

**Export sales (USDA FAS ESR, weekly)**

| Tool | What it does |
|------|--------------|
| `get_export_sales` | Latest week's contracted export sales by destination country |
| `get_export_sales_history` | Weekly export sales time series, optionally for one destination |
| `list_esr_commodities` | Lists tracked commodities: wheat, barley, corn, sorghum, soybeans, soybean meal, soybean oil, cotton, rice |

**Trade flows (USDA GATS / Census, monthly)**

| Tool | What it does |
|------|--------------|
| `get_trade_flows` | Actual shipped export quantities and values by destination (24 rolling months) |
| `compare_contracts_vs_shipments` | Contracted (ESR) vs shipped (GATS) gap — the unshipped export backlog in metric tons |
| `list_gats_commodities` | Lists commodities tracked in trade flows (HS-chapter matched) |

**NASS statistics (USDA QuickStats)**

| Tool | What it does |
|------|--------------|
| `get_crop_stats` | Annual yield, production, planted/harvested area for a commodity, national or state |
| `get_crop_condition` | Latest weekly crop condition ratings (excellent → very poor) for corn, soybeans, wheat |
| `get_crop_progress` | Latest weekly crop progress by growth stage for corn, soybeans, wheat |
| `get_crop_condition_history` | Week-by-week condition ratings across the growing season |
| `get_crop_progress_history` | Week-by-week progress across the growing season |

**Weather (NOAA, live proxy)**

| Tool | What it does |
|------|--------------|
| `get_weather` | Current observed conditions for an ag region from the nearest NWS station |
| `get_forecast` | NOAA gridpoint forecast for an ag region (day/night periods) |
| `get_weather_history` | Recent past weather by day: highs, lows, precipitation |

**Analytics (computed from stored USDA data)**

| Tool | What it does |
|------|--------------|
| `get_spreads` | Corn-to-nitrogen cost per fertilizer product, soybean/corn ratio, feeder-to-fed cattle spread |

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

```bash
curl "https://ag.morningbrief.link/v1/grain/prices?product=corn&geography=IL&limit=1"
curl "https://ag.morningbrief.link/v1/wasde/projections?commodity=corn&limit=1"
```

Full documentation: https://morningbrief.link/docs

### Sample response: grain prices

`GET /v1/grain/prices?limit=1` returns USDA cash bids with provenance:

```json
{
  "as_of": "2026-10-22",
  "count": 1,
  "prices": [
    {
      "product": "barley",
      "label": "Barley",
      "geography": "MT",
      "price": 3.64,
      "unit": "USD/bu",
      "date": "2026-10-22",
      "change": 0,
      "change_pct": null
    }
  ],
  "_provenance": {
    "as_of": "2026-10-22",
    "source": "USDA AMS",
    "source_detail": "MARS API",
    "cadence": "daily",
    "cadence_detail": "Weekday market reports",
    "fetched_at": "2026-10-23T00:35:13.000Z"
  }
}
```

### Sample response: WASDE projections

`GET /v1/wasde/projections?commodity=corn&limit=1` returns balance-sheet rows:

```json
{
  "as_of": "2026-10-18",
  "market_year": 2026,
  "country": "WORLD",
  "count": 1,
  "projections": [
    {
      "commodity": "corn",
      "label": "Corn",
      "country": "WORLD",
      "market_year": 2026,
      "attribute": "beginning_stocks",
      "attribute_label": "Beginning Stocks",
      "value": 307162,
      "unit": "(1000 MT)",
      "report_date": "2026-10-18"
    }
  ],
  "_provenance": {
    "as_of": "2026-10-18",
    "source": "USDA FAS",
    "source_detail": "PSD API (WASDE)",
    "cadence": "monthly",
    "cadence_detail": "Monthly release, captured mid-month",
    "fetched_at": "2026-10-18T06:02:05.000Z"
  }
}
```

## Use cases

- **Grain marketing** — compare today's Illinois corn cash price against its 30-day trend and the WASDE outlook before selling stored grain.
- **Input cost planning** — track anhydrous ammonia and urea prices against corn prices with `get_spreads` to time fertilizer purchases.
- **Export demand watch** — see who is buying US corn this week (`get_export_sales`) and whether it has actually shipped (`compare_contracts_vs_shipments`).
- **Crop outlooks** — pair weekly NASS crop condition ratings with WASDE supply revisions for a data-driven harvest outlook.
- **Livestock feeding economics** — relate feeder-to-fed cattle spreads to feed grain prices when running feeding margins.
- **Agent research** — let an AI agent pull authoritative USDA figures into market summaries without scraping, keys, or rate limits.

## Links

- Hub: https://morningbrief.link
- Product page: https://morningbrief.link/ag/
- Docs: https://morningbrief.link/docs
- MCP Registry: search for `link.morningbrief/magpie`

## Cost & access

Free, no API key required.
