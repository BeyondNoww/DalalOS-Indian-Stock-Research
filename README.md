# Indian stock market (NSE/BSE) MCP server

**DalalOS is a hosted Indian stock market (NSE/BSE) MCP server.** It gives Claude, ChatGPT, Cursor and other MCP clients 59 tools for company and market data: quotes, financials and ratios, shareholding, corporate filings (facts with the source page and verbatim quote), order book and other operating metrics for the companies that disclose and are indexed, screens, FII/DII flows, F&O positioning and macro series. Each response carries its source and a freshness date. It returns reported facts and mechanically-computed ratios, never a buy/sell call.

- MCP endpoint (streamable HTTP): `https://mcp.dalalos.in/mcp`
- Website: https://dalalos.in
- MCP Registry name: `in.dalalos/mcp`
- Free to start, no card: https://dalalos.in/pricing

> **Not investment advice.** DalalOS provides market data and mechanically-computed measures from public filings and exchanges. It does not issue recommendations, target prices or trading signals, and it is not a SEBI-registered investment advisor. See [boundaries](#boundaries).

## Connect

DalalOS uses OAuth: add the URL, sign in once in the browser window your client opens, and you are on the free tier. No API key is needed for the clients below. The server URL is always:

```text
https://mcp.dalalos.in/mcp
```

### Claude (claude.ai and Claude Desktop)

Open **Settings → Connectors → Add custom connector**, paste the URL above, choose **Connect**, then complete the DalalOS sign-in window and start a new chat.

### Claude Code

```bash
claude mcp add --transport http dalalos https://mcp.dalalos.in/mcp
```

Then run `/mcp` inside Claude Code to finish the browser sign-in.

### ChatGPT

The easiest way is the published plugin: [open DalalOS in ChatGPT](https://chatgpt.com/plugins/plugin_asdk_app_6aded6de90f081918717da5227322e16), select **Install plugin**, and sign in to DalalOS when prompted (Google, or a code we email you). It is published for ChatGPT users in India.

If the plugin is not available to you, add the server manually. ChatGPT does not use a JSON config file for this: in ChatGPT open **Plugins → Create Plugin**, choose **Server URL**, enter `https://mcp.dalalos.in/mcp`, select **OAuth**, create the plugin and complete the DalalOS sign-in window. Then start a new chat and select the plugin.

### Cursor

Add to `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global), then choose OAuth when Cursor offers it:

```json
{
  "mcpServers": {
    "dalalos": {
      "url": "https://mcp.dalalos.in/mcp"
    }
  }
}
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "dalalos": {
      "type": "http",
      "url": "https://mcp.dalalos.in/mcp"
    }
  }
}
```

### Codex

```bash
codex mcp add dalalos --url https://mcp.dalalos.in/mcp
codex mcp login dalalos
```

### Any other MCP client

Use the client's own schema for a remote streamable-HTTP server. The generic shape is:

```json
{
  "mcpServers": {
    "dalalos": {
      "url": "https://mcp.dalalos.in/mcp"
    }
  }
}
```

Only if a client cannot complete OAuth and requires a Bearer header, create an API key in your DalalOS account and send `Authorization: Bearer <your DalalOS API key>`. Never commit or share a key. More client guides (Windsurf, Zed, Kiro, OpenCode): https://dalalos.in/connect.

### Check it works

> Use DalalOS to fetch the latest available quote and company profile for Reliance Industries. Tell me which DalalOS tools you called, the source of the data, and its freshness date. Do not use web search.

A working connection names the DalalOS tools it called and returns source and freshness metadata.

## Example prompts

- "What is Reliance trading at, and where is it versus its 52-week range? Include the data source and freshness timestamp."
- "Show TCS revenue, margins, ROCE and debt-to-equity for the last five years."
- "Compare Titan and Kalyan Jewellers on margins and debt-to-equity."
- "Find IT-sector stocks with ROE over 20% and PE under 30."
- "Screen for ROCE above 22% and either debt-to-equity under 0.5 or interest coverage above 8." (free-form screener query)
- "Which companies announce results in the next two weeks?"
- "What has Larsen & Toubro disclosed about its order book and order inflow? Show the filings behind each figure."
- "Summarise the latest board-meeting filing from Infosys and quote the lines from the PDF it is based on."
- "Has promoter pledge in this company changed over the last year, and are there recent insider-trading or pledge disclosures?"
- "Which sectors have foreign portfolio investors been net buying and selling in the latest fortnight?"

Prompt pattern for when recency matters: *"Name the DalalOS tools used, show the source, the cached date and any stale or coverage warning. Do not substitute web search for a missing DalalOS value."* More: [`examples/PROMPTS.md`](examples/PROMPTS.md).

## What is different

- **First-party and filing-grounded.** Financials come from SEBI-mandated BSE-XBRL filings; exchange filings are extracted into facts that keep their source page and verbatim quotes.
- **Source and freshness on every response.** Each result carries its provenance, a `cached_at` time and a `stale` flag. When freshness is the question there is a dedicated `get_data_freshness` tool.
- **Honest gaps.** Missing, not-applicable and stale data are surfaced as such, not replaced with an invented value. A valid zero and missing data are different things.
- **No verdicts.** Raw-as-reported figures or mechanically-computed ratios only: no buy/sell calls, target prices or "top picks".

## Tools

DalalOS exposes **59 current MCP tools**. The live `tools/list` returned 61 entries on 27 October 2026: those 59, a `ping` helper and one deprecated alias (`get_fii_activity`); the maintained reference is https://dalalos.in/tools and the machine-readable list is [`capabilities.json`](capabilities.json).

| Group | Count | Tools |
| --- | ---: | --- |
| Search and discovery | 9 | `search_stocks`, `screen_stocks`, `screen_by_query`, `list_preset_screens`, `run_preset_screen`, `get_sector_overview`, `list_indices`, `list_index_constituents`, `get_market_heatmap` |
| Quotes and prices | 6 | `get_stock_quote`, `get_batch_quotes`, `get_price_history`, `get_returns`, `get_delivery_trends`, `get_technical_indicators` |
| Financials and ratios | 7 | `get_financials`, `get_financial_trends`, `get_company_profile`, `get_company_report`, `get_company_operating_metrics`, `get_valuation_history`, `get_brsr` |
| Peer comparison | 2 | `get_peer_comparison`, `get_relevant_peers` |
| Ownership and risk | 3 | `get_shareholding`, `get_pledge_trend`, `get_company_exposures` |
| Corporate actions, events and filings | 12 | `get_stock_events`, `get_filing_extract`, `get_filing_extracts`, `get_event_context`, `search_company_disclosures`, `list_announcement_buckets`, `get_announcement_bucket`, `get_corporate_actions`, `get_dividends`, `get_upcoming_results`, `get_market_calendar`, `get_ipos` |
| Market context | 16 | `get_top_movers`, `get_market_breadth`, `get_india_vix`, `get_index_valuation`, `list_index_valuations`, `get_market_fii_dii_flow`, `get_sector_fpi_investment`, `get_sector_fii_flow` (historical), `get_stock_fii_flow` (historical), `get_bulk_block_deals`, `get_derivatives_positioning`, `get_fo_participant_activity`, `get_fno_ban_list`, `get_macro_indicators`, `get_fx_reference_rates`, `get_company_macro_exposures` |
| Watchlist | 3 | `get_watchlist`, `add_to_watchlist`, `remove_from_watchlist` |
| System | 1 | `get_data_freshness` |

Read and write behaviour:

- Market-data tools read a maintained cache; nothing is scraped from an exchange during your tool call.
- Filing tools (`get_stock_events`, `get_filing_extract`, `get_filing_extracts`, `get_announcement_bucket`) can queue filing processing and use a small on-demand allowance, so they are not marked read-only.
- `add_to_watchlist` and `remove_from_watchlist` only change your own DalalOS watchlist. DalalOS is not a broker and cannot place orders.

## Data sources

| Source | Used for |
| --- | --- |
| NSE archives | End-of-day prices and traded-market fields, index and exchange-published datasets |
| BSE official interfaces and filings | Security master, corporate announcements, filings, corporate actions |
| SEBI-mandated BSE-XBRL filings | Reported financial statements and the ratios computed from them |
| NSDL fortnightly sector-wise FPI report | Sector-level foreign portfolio investment |
| MOSPI, PPAC, FBIL | Macro indicators, crude basket and USD/INR reference rates |

Quote-style data is end-of-day, not live ticks. Freshness differs by domain: prices refresh after an exchange session is published, filings arrive when the exchange publishes them, ownership follows its reporting cadence. Details: [`docs/DATA_SOURCES.md`](docs/DATA_SOURCES.md) and https://dalalos.in/docs/data-sources-and-freshness.

### Response envelope

Tool responses share one envelope: `status`, `data`, `source`, `cached_at`, `stale`, plus `error` and a `reason_code` when something is unavailable, not applicable or incomplete. Surface `source` and `cached_at` when you quote a figure.

## Pricing

Free to start with a monthly credit allowance, no card and no waitlist. Paid plans (Plus and Pro) add higher limits, and Pro includes REST access. Current allowances and prices: https://dalalos.in/pricing.

## Boundaries

- **Not real-time:** quote-style data is end-of-day, not a tick or order-book feed.
- **Research, not execution:** not a broker; no orders or trades.
- **No investment advice:** no buy/sell calls, target prices or recommendations.
- **Account writes are limited** to your own DalalOS watchlist.
- **Not affiliated** with or endorsed by NSE, BSE, SEBI, Anthropic, OpenAI, Cursor or any other client or data vendor.

## About this repository

This is the public project, integration and discovery surface for DalalOS. It holds connection documentation, the registry manifest, the sanitized capability list and example prompts. It intentionally does not contain the DalalOS backend: no ingestion code, schemas, deployment details or credentials.

| File | Purpose |
| --- | --- |
| [`server.json`](server.json) | Official MCP Registry manifest (`in.dalalos/mcp`) |
| [`capabilities.json`](capabilities.json) | Public tool names with read/write and deprecation flags |
| [`glama.json`](glama.json), [`smithery.yaml`](smithery.yaml) | Directory metadata for Glama and Smithery |
| [`docs/CONNECT.md`](docs/CONNECT.md) | Connection and authentication notes |
| [`docs/DATA_SOURCES.md`](docs/DATA_SOURCES.md) | Provenance and freshness contract |
| [`docs/DIRECTORY_LISTING.md`](docs/DIRECTORY_LISTING.md) | Canonical copy for MCP directories and catalogs |
| [`docs/OPENAI_PLUGIN_SUBMISSION.md`](docs/OPENAI_PLUGIN_SUBMISSION.md) | OpenAI Plugin listing and review notes |
| [`examples/PROMPTS.md`](examples/PROMPTS.md) | Example research prompts and out-of-scope requests |
| [`CONTRIBUTING.md`](CONTRIBUTING.md), [`SECURITY.md`](SECURITY.md) | Contribution guardrails and security reporting |

Privacy: https://dalalos.in/privacy. Terms: https://dalalos.in/terms. Questions: https://dalalos.in/contact.

The documentation and metadata in this repository are MIT licensed ([`LICENSE`](LICENSE)). The hosted service and the data it returns are governed by the DalalOS terms, not by that licence; see [`NOTICE`](NOTICE).
