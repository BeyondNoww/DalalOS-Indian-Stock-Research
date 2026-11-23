# Screen Indian stocks in plain English with Claude or ChatGPT

How to describe a stock screen in words and have Claude or ChatGPT run it on NSE and BSE companies with the DalalOS MCP server.

You say what you want, for example "companies with ROCE above 20% and debt to equity below 0.5". The assistant turns it into a screen and runs it on the DalalOS server, so the numbers do not come from the model's memory.

## Connect

Add `https://mcp.dalalos.in/mcp` as a connector and sign in once. Steps for each client are in the [README](../../README.md#connect). The free tier needs no card.

## The four screening tools

| Tool | Use it for |
| --- | --- |
| `screen_stocks` | Plain numeric filters combined with AND, plus exact-match filters such as sector, company type and index |
| `screen_by_query` | One expression with OR, arithmetic between ratios and functions. Takes an optional scope (index, sector, industry) |
| `list_preset_screens` | The ready-made screens and when each was computed |
| `run_preset_screen` | The stored result of one preset |

## Example prompts

> Screen NSE and BSE companies with ROCE above 20% and debt to equity below 0.5. Show the first 25 by market cap, print the exact filters you used, and tell me how many companies matched and how many were excluded for a missing ratio.

> Find companies where PE is below 25 and ROE is above 15%, or where PE is below 15 and ROE is above 10%. Use a query with OR and show me the query.

> Within the NIFTY 50, which companies have promoter pledge above zero? Show the pledged percentage for each.

> List the ready-made screens you can run, with the date each was computed. Then run the one for low debt and high ROE.

A plain filter screen cannot say OR, so the second prompt needs `screen_by_query`.

## Check what the assistant ran

1. Ask for the exact filters or query text and compare each threshold with what you meant.
2. Check the units. In `screen_stocks`, ROE, ROCE and margins are fractions (0.15 is 15 percent), and promoter and pledged percentages are percentages. In the default query dialect of `screen_by_query`, `22%` means 22 percent.
3. Ask how many companies matched, how many were scanned and how many were dropped for a missing ratio.
4. For a company you care about, read the filed results before relying on a computed ratio.

## Limits

- Screens read metrics computed each night. They do not reflect a filing or price that came after the last computation.
- A condition on a missing value does not match, so a company without that ratio drops out. Ratios such as ROCE and interest coverage are empty for banks, NBFCs and insurers.
- A screen lists every company that matches, sorted by a column you pick (market cap by default). The order is a sort, not a ranking.
- No buy or sell calls, target prices or ratings.

Units, dialects, paging and the dated checks are on the website: https://dalalos.in/use-cases/indian-stock-screener-mcp and https://dalalos.in/docs/screener.

This is research data, not investment advice.
