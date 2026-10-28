# Indian stock market (NSE/BSE) MCP server

**DalalOS is a hosted Indian stock market (NSE/BSE) MCP server.** It gives Claude, ChatGPT, Cursor, Claude Code, Windsurf and other MCP clients 59 tools for company and market data: quotes, financials and ratios, shareholding and promoter pledge, corporate filings (facts with the source page and verbatim quote), order book and other operating metrics for the companies that disclose and are indexed, screens, FII/DII flows, F&O positioning, IPOs, corporate actions and macro series. Each response carries its source and a freshness date. It returns reported facts and mechanically-computed ratios, never a buy/sell call.

You do not need a brokerage account, a data-terminal subscription or an API key. Add the URL to your client, sign in once with Google or an emailed code, and you are on the free tier.

- MCP endpoint (streamable HTTP): `https://mcp.dalalos.in/mcp`
- Website: https://dalalos.in
- MCP Registry name: `in.dalalos/mcp`
- PyPI: [`dalalos-mcp`](https://pypi.org/project/dalalos-mcp/) (connection helper and stdio proxy)
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

### Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`, restart Windsurf, then complete the DalalOS sign-in when it opens:

```json
{
  "mcpServers": {
    "dalalos": {
      "serverUrl": "https://mcp.dalalos.in/mcp"
    }
  }
}
```

Windsurf names the remote-URL field `serverUrl`, not `url`.

### Cline

Add to `cline_mcp_settings.json` (Cline's MCP settings). This form uses a DalalOS API key from https://dalalos.in/dashboard/keys:

```json
{
  "mcpServers": {
    "dalalos": {
      "type": "streamableHttp",
      "url": "https://mcp.dalalos.in/mcp",
      "headers": { "Authorization": "Bearer <your DalalOS API key>" }
    }
  }
}
```

### Command-only clients (uvx or pip)

Clients that can only launch a local command, such as the Claude Desktop config file, can run the stdio proxy from the [`dalalos-mcp`](https://pypi.org/project/dalalos-mcp/) package. It forwards to the hosted server and needs an API key from https://dalalos.in/dashboard/keys:

```json
{
  "mcpServers": {
    "dalalos": {
      "command": "uvx",
      "args": ["dalalos-mcp", "proxy"],
      "env": { "DALALOS_API_KEY": "<your DalalOS API key>" }
    }
  }
}
```

Without `uv`, run `pip install dalalos-mcp` and use `"command": "dalalos-mcp"` with the same `args` and `env`. Two helper commands: `uvx dalalos-mcp check` confirms the hosted endpoint answers, and `uvx dalalos-mcp config <client>` prints a connection snippet for `claude`, `claude-code`, `codex`, `chatgpt`, `cursor`, `generic` or `stdio`.

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

Only if a client cannot complete OAuth and requires a Bearer header, create an API key in your DalalOS account and send `Authorization: Bearer <your DalalOS API key>`. Never commit or share a key. More client guides (Zed, Kiro, OpenCode): https://dalalos.in/connect.

### Check it works

> Use DalalOS to fetch the latest available quote and company profile for Reliance Industries. Tell me which DalalOS tools you called, the source of the data, and its freshness date. Do not use web search.

A working connection names the DalalOS tools it called and returns source and freshness metadata.

## Example prompts

Each prompt names the DalalOS tools your assistant should reach for. You do not need to name them; the assistant picks them from the tool descriptions.

| Topic | Example prompt | Tools |
| --- | --- | --- |
| Quotes | "What is Reliance trading at, and where is it versus its 52-week range? Include the data source and freshness timestamp." | `get_stock_quote`, `get_returns` |
| Company filings | "What has Infosys filed with the exchange in the last 30 days? Summarise the latest board-meeting filing and quote the lines from the PDF it is based on." | `get_stock_events`, `get_filing_extract` |
| Past disclosures | "Find the earlier filing where Larsen & Toubro disclosed a prior order value." | `search_company_disclosures`, `get_event_context` |
| Order book and inflow | "What has Larsen & Toubro disclosed about its order book and order inflow? Show the filing behind each figure." | `get_company_operating_metrics`, `get_filing_extract` |
| Quarterly results | "Show TCS quarterly revenue and profit for the last eight quarters, and the quarter-on-quarter margin trend." | `get_financials`, `get_financial_trends` |
| Upcoming results | "Which companies announce results in the next two weeks?" | `get_upcoming_results` |
| Ratios | "Show TCS ROCE, ROE, P/E and debt-to-equity for the last five years." | `get_financials`, `get_valuation_history` |
| Screener | "Find IT-sector stocks with ROE over 20% and PE under 30." For OR logic: "ROCE above 22% and either debt-to-equity under 0.5 or interest coverage above 8." | `screen_stocks`, `screen_by_query`, `run_preset_screen` |
| Peers | "Compare Titan and Kalyan Jewellers on margins and debt-to-equity, and tell me which companies DalalOS treats as Titan's peers." | `get_peer_comparison`, `get_relevant_peers` |
| Shareholding and pledge | "Has promoter pledge in this company changed over the last year? Show promoter, FII, DII and mutual fund holding by quarter." | `get_shareholding`, `get_pledge_trend` |
| Corporate actions and dividends | "List ITC's dividends and corporate actions for the last five years." | `get_corporate_actions`, `get_dividends` |
| FII and DII flows | "What have FIIs and DIIs bought and sold in the cash market over the last ten sessions? Which sectors have foreign portfolio investors been net buying in the latest fortnight?" | `get_market_fii_dii_flow`, `get_sector_fpi_investment` |
| F&O positioning | "Show NIFTY open interest and put-call ratio, FII and client positioning in index futures, and today's F&O ban list." | `get_derivatives_positioning`, `get_fo_participant_activity`, `get_fno_ban_list` |
| IPOs | "Which IPOs are open or upcoming?" | `get_ipos` |
| Management commentary | "What did management say in the latest results filing about the outlook? Quote the lines and give the source page." | `get_stock_events`, `get_filing_extract` |
| Macro | "Show the latest CPI, WPI, IIP and crude-basket readings and the FBIL USD/INR reference rate." | `get_macro_indicators`, `get_fx_reference_rates` |
| Raw data only | "Show the facts and ratios for this company without any buy or sell view." | any tool: DalalOS returns reported figures and computed ratios only |

Prompt pattern for when recency matters: *"Name the DalalOS tools used, show the source, the cached date and any stale or coverage warning. Do not substitute web search for a missing DalalOS value."* More: [`examples/PROMPTS.md`](examples/PROMPTS.md).

## What is different

- **Official sources only.** Data comes from NSE and BSE publications, SEBI-mandated BSE-XBRL filings, NSDL, MOSPI, PPAC and FBIL. DalalOS does not use third-party data-vendor feeds.
- **First-party and filing-grounded.** Financials come from SEBI-mandated BSE-XBRL filings; exchange filings are extracted into facts that keep their source page and verbatim quotes.
- **Research only, no brokerage account.** Nothing connects to a broker and nothing places an order. The only write is your own DalalOS watchlist.
- **Source and freshness on every response.** Each result carries its provenance, a `cached_at` time and a `stale` flag. When freshness is the question there is a dedicated `get_data_freshness` tool.
- **Honest gaps.** Missing, not-applicable and stale data are surfaced as such, not replaced with an invented value. A valid zero and missing data are different things.
- **No verdicts.** Raw-as-reported figures or mechanically-computed ratios only: no buy/sell calls, target prices or "top picks".

## Tools

DalalOS exposes **59 current MCP tools**. The live `tools/list` returned 61 entries on 29 October 2026 (server version 1.28.1): those 59, a `ping` helper and one deprecated alias (`get_fii_activity`, use `get_stock_fii_flow`). Directory sites that count every name the server lists, such as Glama, show 61. [`capabilities.json`](capabilities.json) lists 60 (the 59 plus the deprecated alias) and leaves out the `ping` health check. The maintained reference is https://dalalos.in/tools.

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
