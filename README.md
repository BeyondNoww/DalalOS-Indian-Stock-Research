# DalalOS — Indian stock research for AI agents

DalalOS is a hosted Model Context Protocol (MCP) data layer for researching Indian NSE/BSE-listed companies with AI assistants and agents.

- Website: https://dalalos.in
- MCP endpoint: `https://mcp.dalalos.in/mcp`
- Connection guide: https://dalalos.in/connect
- Tool reference: https://dalalos.in/tools
- Data sources & freshness: https://dalalos.in/docs/data-sources-and-freshness

## What DalalOS provides

DalalOS exposes structured Indian-market research data to MCP-compatible clients, including:

- company and security search;
- end-of-day quotes and price history;
- company financial statements and mechanically-computed ratios;
- peer and sector comparison data;
- shareholding and promoter-pledge data;
- corporate actions, results, filings and disclosed events;
- screening, indices and market context;
- official macro/reference datasets used for research context; and
- an account-scoped DalalOS watchlist.

Data is sourced from official and public Indian-market sources such as NSE archives, BSE official interfaces and exchange filings, SEBI-mandated BSE-XBRL filings, and the named official public-sector publisher for supported macro/reference datasets.

## Important boundaries

- **Not real-time:** quote-style stock-market data is end-of-day, not a live tick or order-book feed.
- **Research, not execution:** DalalOS is not a broker and does not place market orders or execute trades.
- **No investment advice:** DalalOS returns sourced facts and mechanically-computed measures, not buy/sell calls, target prices or investment recommendations.
- **Read/write scope:** market-data research tools are read-only. The explicit write exception is the caller's own DalalOS watchlist, where authenticated users can add or remove symbols.
- **Missing data stays explicit:** unsupported, unavailable or stale coverage is surfaced rather than replaced with invented values.

## Connect

Use the hosted streamable-HTTP endpoint:

```text
https://mcp.dalalos.in/mcp
```

OAuth is the preferred authentication path for clients that support it. API-key Bearer authentication is available as a fallback for compatible clients that require it.

See [`docs/CONNECT.md`](docs/CONNECT.md) for public-safe setup notes and the maintained website guides for client-specific instructions.

## MCP Registry identity

The public registry manifest is [`server.json`](server.json). DalalOS already has an official MCP Registry identity under:

```text
io.github.aatharva16/dalalos
```

That existing identity is intentionally preserved so the hosted endpoint has one canonical Registry record. Registry metadata changes must be published using credentials authorized for that existing namespace; this public repository does not store those credentials.

## Repository purpose

This repository is the **public project, integration and discovery surface** for DalalOS. It exists so users, MCP registries, AI-tool directories and open-source ecosystem lists have a stable public GitHub reference for the hosted service.

It intentionally does **not** contain the DalalOS backend implementation. In particular, this repository does not publish internal ingestion logic, database/cache schemas, source-adapter code, deployment topology, credentials, private operational tooling, private issue/PR history, or unpublished architecture.

Public files in this repository are limited to integration metadata, connection documentation, public capability/provenance descriptions and safe usage examples.

## Public metadata

- [`server.json`](server.json) — MCP Registry metadata for the hosted server.
- [`docs/CONNECT.md`](docs/CONNECT.md) — connection and authentication notes.
- [`docs/DATA_SOURCES.md`](docs/DATA_SOURCES.md) — provenance and freshness contract.
- [`examples/PROMPTS.md`](examples/PROMPTS.md) — example research prompts and capability-boundary examples.
- [`SECURITY.md`](SECURITY.md) — responsible security reporting guidance.

## Legal and privacy

- Privacy: https://dalalos.in/privacy
- Terms: https://dalalos.in/terms

DalalOS is not affiliated with or endorsed by NSE, BSE, SEBI, OpenAI, Anthropic, Cursor, or another AI-client vendor unless explicitly stated otherwise by that party.
