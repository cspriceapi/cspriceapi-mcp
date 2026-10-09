# CSPriceAPI MCP Server — live CS2 skin prices for AI assistants

CSPriceAPI is the CS2 skin price API for traders who need Chinese markets: live BUFF163 and YouPin prices, YouPin buy orders, Doppler phases and float-ranged prices across 13 marketplaces in one REST API.

This remote [Model Context Protocol](https://modelcontextprotocol.io) server lets Claude, ChatGPT, Cursor, VS Code Copilot and other MCP clients look up those prices directly.

```
https://api.cspriceapi.com/mcp
```

Transport: Streamable HTTP. No install needed. Docs: [cspriceapi.com/docs/mcp](https://cspriceapi.com/docs/mcp)

## What you can ask

- "What does a Karambit Doppler Factory New cost on BUFF163 and YouPin, per phase?"
- "Which marketplace is cheapest for an AK-47 Redline Field-Tested right now?"
- "How far is the best YouPin buy order below the lowest listing for the M4A1-S Printstream MW?"
- "Show the 90-day YouPin price history of the AWP Asiimov Field-Tested."

## Tools

| Tool | Returns | Access |
|---|---|---|
| `find_items` | Exact `market_hash_name` values for a loose query ("karambit doppler fn", "st m4a1 printstream") | Free preview |
| `get_prices` | Lowest listing price, listing count and an approximate USD value on YouPin898, BUFF163, C5Game, EcoSteam, SkinBaron, Skinport, CSFloat, LIS-SKINS, DMarket, ShadowPay, Waxpeer and Market.CSGO, the cheapest market, Steam best ask/bid. Doppler names return every phase | Free preview / Basic |
| `get_youpin_buy_orders` | Best YouPin buy order; with filters, every order with its float range, phase, Fade or Case Hardened tier | Pro / Enterprise |
| `get_float_ranged_prices` | YouPin lowest price per float range, with phase and Fade % | Pro |
| `get_price_history` | Daily or hourly price history on one marketplace | Enterprise |
| `get_recent_sales` | Recent CSFloat, YouPin and Skinport sales with float, pattern and stickers | Enterprise |

**Free preview (no key):** `find_items` and `get_prices` for one item per call, 5 lookups per day, prices from a snapshot up to 6 hours old.
**With an API key:** live prices (refreshed about every 5 minutes), up to 50 items per call, and every tool your plan includes. Plans: [cspriceapi.com/pricing](https://cspriceapi.com/pricing). A free 2-day test key is available in the `#get-testkey` channel on [Discord](https://discord.gg/XT3rpfH4Tx).

Send the key as `x-api-key: YOUR_API_KEY` or `Authorization: Bearer YOUR_API_KEY`.

## Setup

### Claude Code

```bash
claude mcp add --transport http cspriceapi https://api.cspriceapi.com/mcp \
  --header "x-api-key: YOUR_API_KEY"
```

### Claude Desktop

```json
{
  "mcpServers": {
    "cspriceapi": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://api.cspriceapi.com/mcp", "--header", "x-api-key:${CSPRICEAPI_KEY}"],
      "env": { "CSPRICEAPI_KEY": "YOUR_API_KEY" }
    }
  }
}
```

On claude.ai, add `https://api.cspriceapi.com/mcp` as a custom connector (free preview; custom connectors cannot send a key header).

### Cursor

```json
{
  "mcpServers": {
    "cspriceapi": {
      "url": "https://api.cspriceapi.com/mcp",
      "headers": { "x-api-key": "YOUR_API_KEY" }
    }
  }
}
```

### VS Code

```json
{
  "inputs": [
    { "type": "promptString", "id": "cspriceapi-key", "description": "CSPriceAPI key", "password": true }
  ],
  "servers": {
    "cspriceapi": {
      "type": "http",
      "url": "https://api.cspriceapi.com/mcp",
      "headers": { "x-api-key": "${input:cspriceapi-key}" }
    }
  }
}
```

### ChatGPT

Settings → Apps & Connectors → developer mode → Create, URL `https://api.cspriceapi.com/mcp`, no authentication (free preview).

## Notes

- Read-only: the server cannot buy, sell or access any marketplace account.
- Prices are in each marketplace's own currency (CNY, EUR or USD); `price_usd` is an approximate conversion at ECB rates.
- The same data is available as a REST API: [docs](https://cspriceapi.com/docs/introduction), [OpenAPI](https://cspriceapi.com/openapi.yaml).
- Support and feature requests: [Discord](https://discord.gg/XT3rpfH4Tx) or support@cspriceapi.com.
