# DalalOS directory listing kit

Use this page when adding or updating DalalOS in an MCP directory, AI-tool catalog, developer resource list or finance-data roundup. The prose is deliberately capability-stable: it does not hard-code a tool count that can drift as the hosted service evolves.

## Canonical identity

- **Name:** DalalOS
- **Category:** Indian stock-market data / financial research / MCP server
- **Website:** https://dalalos.in
- **Public GitHub project:** https://github.com/BeyondNoww/DalalOS-Indian-Stock-Research
- **Hosted MCP endpoint:** `https://mcp.dalalos.in/mcp`
- **Official MCP Registry identity:** `io.github.aatharva16/dalalos`
- **Machine-readable capabilities:** https://github.com/BeyondNoww/DalalOS-Indian-Stock-Research/blob/main/capabilities.json
- **Connection guide:** https://dalalos.in/connect
- **Tool reference:** https://dalalos.in/tools
- **Data sources & freshness:** https://dalalos.in/docs/data-sources-and-freshness

## One-line description

> Hosted MCP data layer for source-backed Indian NSE/BSE stock research with AI assistants and agents.

## Short description

> DalalOS connects MCP-compatible AI clients to sourced Indian NSE/BSE research data, including end-of-day prices, financials and mechanically-computed ratios, ownership, disclosures, corporate actions, screening, indices and market context. Market-data tools are read-only; the caller's account-scoped DalalOS watchlist is the explicit add/remove write exception. DalalOS is not a broker, live tick feed or investment-advice service.

## Capability counts

When a directory requires an exact current tool count, read it from [`../capabilities.json`](../capabilities.json) rather than copying a number from prose:

- use `preferred_tool_count` for the current non-deprecated capability count;
- use `registered_tool_count` only when the directory explicitly wants every registered MCP name, including compatibility aliases;
- do not count deprecated aliases as separate preferred product capabilities;
- use the per-tool `read_only` / `destructive` flags to preserve the market-data-read versus account-watchlist-write distinction.

## Suggested tags

`mcp`, `model-context-protocol`, `finance`, `financial-data`, `stock-market`, `india`, `nse`, `bse`, `market-data`, `ai-agents`, `stock-research`, `streamable-http`

## Authentication

- Preferred where supported: OAuth.
- Fallback for compatible clients that require it: DalalOS API key via Bearer authentication.
- Never include a real API key or credential in a public directory listing.

## Data/freshness wording

Use these statements when a directory asks about data quality or latency:

- Quote-style stock-market data is end-of-day, not real-time tick/order-book data.
- DalalOS uses official and public Indian-market sources appropriate to each supported data domain.
- Results expose source/freshness or coverage context where applicable; unavailable data should not be represented as an invented value.
- Financial and market outputs are reported facts or mechanically-computed measures, not buy/sell recommendations or target prices.

## Example prompts

- “Show the latest available end-of-day quote for RELIANCE with its source and freshness date.”
- “Compare HDFC Bank with relevant peers using sourced valuation and operating metrics.”
- “Show the latest shareholding pattern and promoter-pledge trend for this company.”
- “Screen Indian IT stocks using the raw filters I specify; do not turn the results into investment recommendations.”

More examples: [`../examples/PROMPTS.md`](../examples/PROMPTS.md).

## Do not use stale claims

Do not copy an old hard-coded tool count, claim real-time prices, describe DalalOS as a trading/execution service, or imply that it issues investment recommendations. When an external listing disagrees with this page or the maintained website, use the current public DalalOS documentation and `capabilities.json` as the public discovery references and update the external listing.
