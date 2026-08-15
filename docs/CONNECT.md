# Connect DalalOS

DalalOS is hosted at a single streamable-HTTP MCP endpoint:

```text
https://mcp.dalalos.in/mcp
```

## Authentication

Use **OAuth** when your MCP client supports a browser-based authorization flow. This is the preferred path because credentials stay out of local config files.

For clients that require an explicit Bearer credential, DalalOS also supports API-key authentication. Never commit, publish, paste into examples, or share a real API key.

## Client-specific guides

Client UIs and MCP configuration formats change over time, so the maintained DalalOS website is the source of truth for exact setup steps:

- General connection guide: https://dalalos.in/connect
- ChatGPT and compatible clients: https://dalalos.in/docs/chatgpt
- Claude: https://dalalos.in/docs/claude
- REST API: https://dalalos.in/docs/rest-api

For ChatGPT, OpenAI currently supports custom MCP-powered apps through its Apps/Plugins experience on eligible plans and workspaces. Follow the maintained DalalOS guide above rather than relying on an old screenshot or copied UI path.

## Illustrative MCP configuration

Some MCP clients accept a configuration shaped like this:

```json
{
  "mcpServers": {
    "dalalos": {
      "url": "https://mcp.dalalos.in/mcp"
    }
  }
}
```

This is an **illustrative example only**. Use the exact schema documented by your MCP client.

If your client requires an API key in configuration, the relevant HTTP header is conceptually:

```text
Authorization: Bearer <your DalalOS API key>
```

Do not put a real credential in a public repository, issue, screenshot, support message, or prompt.

## Verify the connection

A useful first test is:

> Use DalalOS to fetch the latest available quote and company profile for Reliance Industries. Tell me which DalalOS tools you called, the source of the data, and its freshness date. Do not substitute web search for DalalOS.

A successful result should use the connected DalalOS tools and surface provenance/freshness information. Quote-style market data is end-of-day, not a live tick feed.

## Capability boundaries

DalalOS is a research data service. It does not place exchange orders, act as a broker, provide target prices, or issue buy/sell recommendations. Market-data tools are read-only; authenticated users may add or remove symbols from their own DalalOS watchlist.
