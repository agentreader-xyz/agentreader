# AgentReader

**Web pages and PDFs as clean Markdown or JSON for AI agents, in a fraction of the tokens.**

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

With prepaid credits, add a header: `"headers": { "Authorization": "Bearer ar_..." }`. No key is needed for the free tier.

### Tools

| Tool | What it does |
|---|---|
| `read_url(url, format?, mode?, render?, extract?)` | Read a web page, feed or PDF as clean Markdown or JSON |
| `search(query, limit?, lang?)` | Web search; returns titles, URLs and snippets to read next |
| `get_balance()` | Show remaining free calls and credits |

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
- **Card pack:** $10 for 7,000 reads at https://agentreader.xyz/buy (no expiry)
- **Crypto (x402):** pay per call in USDC on Base, or buy prepaid credit bundles

Only successful reads are charged. Blocked sites, paywalled pages, timeouts and errors are free.

## About this repository

AgentReader is a **hosted service**; its source code is not public. This repository holds documentation only. Questions, bugs or feedback: contact@agentreader.xyz

- Terms: https://agentreader.xyz/terms
- Privacy: https://agentreader.xyz/privacy
