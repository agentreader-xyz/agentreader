# AgentReader

**AgentReader (agentreader.xyz) turns web pages and PDFs into clean Markdown or JSON for AI agents, in a fraction of the tokens.**

Use it instead of a plain fetch when a page is JavaScript-only, behind a cookie wall, a PDF, or too big for your context (a React docs page: ~91,900 tokens of HTML, ~4,100 via AgentReader).

Website: https://agentreader.xyz · MCP: `https://agentreader.xyz/mcp` · Contact: contact@agentreader.xyz

## The problem

An agent's built-in fetch often comes back with something it can't use:

- an empty JavaScript shell (single-page apps that render client-side)
- a cookie or consent wall instead of the content
- raw PDF bytes
- token-heavy HTML full of menus, ads and scripts
- a "200 OK" that is really a bot challenge or a paywall teaser

AgentReader returns the page's actual content as compact Markdown (or JSON with title, author, date, links and tables). Code blocks are kept verbatim. When a site blocks bots or a page is paywalled, you get an honest error instead of junk, and you are **not charged**.

## Connect over MCP (remote, Streamable HTTP)

Server URL:

```
https://agentreader.xyz/mcp
```

**Claude Code**

```bash
claude mcp add --transport http agentreader https://agentreader.xyz/mcp
```

**Cursor and other MCP clients** (e.g. `~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "agentreader": {
      "url": "https://agentreader.xyz/mcp"
    }
  }
}
```

With prepaid credits, add a header: `"headers": { "Authorization": "Bearer ar_..." }`. No key is needed for the free tier. Apps that can't set headers (claude.ai, ChatGPT connectors) can use `https://agentreader.xyz/mcp?api_key=ar_...` as the connector URL; treat that URL like a password.

MCP clients that speak x402 can also pay per call inside the tool call (x402 MCP transport), without a key.

### Tools

| Tool | What it does |
|---|---|
| `read_url(url, format?, mode?, render?, extract?)` | Read a web page, feed or PDF as clean Markdown or JSON |
| `search(query, limit?, lang?)` | Web search; returns titles, URLs and snippets to read next |
| `get_balance()` | Show remaining free reads and credits |
| `report_issue(url, reason, note?)` | Report a read that failed or looks wrong (free) |

MCP Registry name: `xyz.agentreader/agentreader`

## REST API

```bash
curl "https://agentreader.xyz/v1/read?url=https://en.wikipedia.org/wiki/Markdown"
```

```bash
# JSON with title, author, date, word count, links, tables and metadata
curl "https://agentreader.xyz/v1/read?url=https://en.wikipedia.org/wiki/Markdown&format=json"

# Web search
curl "https://agentreader.xyz/v1/search?q=markdown+spec&limit=5"
```

Useful options: `format=markdown|json`, `mode=auto|article|links|full`, `render=auto|never|always` (headless browser for JS-only pages), `extract=title,author,date,price,...` (structured fields from JSON-LD/OpenGraph/meta tags, no LLM).

Full reference: [llms.txt](https://agentreader.xyz/llms.txt) · [OpenAPI](https://agentreader.xyz/openapi.json)

## Pricing

- **Free:** 5 reads per day, no signup, no key
- **Card packs:** $3 for 2,000 reads or $10 for 7,000 reads + VAT where applicable, at https://agentreader.xyz/buy (one-time, no subscription, credits don't expire). The page issues your API key.
- **Crypto (x402):** pay $0.005 per call in USDC on Base (REST or inside the MCP tool call), or buy prepaid credit bundles (500 reads for $1, 5,000 for $8) via `POST https://agentreader.xyz/v1/credits/buy?bundle=500`

Only successful reads are charged. Blocked sites, paywalled pages, timeouts and errors are free.

## About this repository

AgentReader is a **hosted service**; its source code is not public. This repository holds documentation only. Questions, bugs or feedback: contact@agentreader.xyz

- Terms: https://agentreader.xyz/terms
- Privacy: https://agentreader.xyz/privacy
