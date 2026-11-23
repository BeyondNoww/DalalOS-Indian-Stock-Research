# FII and DII data in Claude and ChatGPT

How to get daily FII and DII cash flow, fortnightly sector FPI investment and F&O participant positions inside Claude or ChatGPT with the DalalOS MCP server.

Claude and ChatGPT do not hold these figures. With the DalalOS connector they call a tool, read the figures from it and show a date and a source. Without a connector they search the web, and the page they read changes from one chat to the next.

## Connect

Add `https://mcp.dalalos.in/mcp` as a connector and sign in once. Steps for each client are in the [README](../../README.md#connect). The free tier needs no card.

## Which tool answers which question

"FII and DII data" means four different datasets. Each has one tool.

| You want | Tool | How often | Source |
| --- | --- | --- | --- |
| Market-wide buying and selling in the cash market | `get_market_fii_dii_flow` | Daily | NSE |
| FPI net investment by sector | `get_sector_fpi_investment` | Fortnightly | NSDL |
| F&O open interest by FII, DII, Pro and Client | `get_fo_participant_activity` | Daily | NSE |
| FII and DII holding in one company | `get_shareholding` | Quarterly | BSE |

`get_market_fii_dii_flow` returns one row per trading day with FII and DII buy, sell and net values in rupees crore. Net is buy minus sell. It takes an optional `from_date` and `to_date`, and the window is capped at 366 days.

## Example prompts

> Show market-wide FII and DII cash flows for the last 10 trading days in a table. Give buy, sell and net in rupees crore, and tell me the date of the latest row and the source.

> Show FPI net investment by sector for the latest fortnight. Say which fortnight it covers and which sectors had net outflows.

> Show FII, DII, Pro and Client open interest in index futures for the last 5 sessions. Do not interpret it, just show the figures.

> Show the FII and DII holding percentage of RELIANCE for the last four quarters.

## Check the answer

- Look at the date of the latest row. The data is end-of-day, not live.
- Check the unit. The daily table is in rupees crore.
- Ask which tool the AI called. A daily FII figure by sector or by stock is a warning sign, because the tools that read that feed (`get_sector_fii_flow`, `get_stock_fii_flow`) depend on a CDSL feed that stopped publishing after March 2025.

## Limits

No live intraday flow. No daily FII flow by sector or by stock for current dates. No view on what a number means for a stock: DalalOS returns reported figures and the buy-minus-sell arithmetic, with no buy or sell calls, ratings or target prices.

The dated checks behind these limits are on the website: https://dalalos.in/use-cases/fii-dii-flow-data-mcp-india. The full tool reference is at https://dalalos.in/tools/get-market-fii-dii-flow.

This is research data, not investment advice.
